---
title: 'Understanding autoload_paths and namespaces in Ruby on Rails'
permalink: /2019/07/understanding-autoload-paths-and-namespaces-in-ruby-on-rails/
excerpt: 'Finding myself seeing <em>NameError: uninitialized</em> too often, I took a little time out to truely understand how the Spring preloader conventions in Rails actually works'
tags:
  - Rails
  - Rails 5
  - autoload_paths
  - Namespaces
---

![Image Credit: Ilze Lucero - unsplash.com/@ilzelucero]({{ site.imageurl }}2019/namespaces-header.jpg)
<br><br>
As I work on larger and more complex Rails projects I use namespaces and directory structures to better organise the code. However, I found myself having to add the odd `require_relative` in my unit tests to work around `NameError: uninitialized constant` errors where Rails couldn't automatically find the classes I was referencing in my now neatly organised projects.

The [official documentation](https://guides.rubyonrails.org/autoloading_and_reloading_constants.html) is comprehensive, but to me this subject reads at an intermediate to advanced level - I’m pretty sure I had missed something basic so it was time for a little investigation.

<div class="panel panel-warning">
  <div class="panel-body bg-warning text--muted">
    <em>Skip the investigation and go straight to <a href="#summary" title="skip to the Summary (on this page)">the summary</a></em><i class="fa fa-fw fa-arrow-down"></i>
  </div>
</div>

Using Ruby on Rails we start with a new app, a single `Product` model, and a `PriceCalculator` whose location we'll play with: <span class="text--muted">*(I'm using Rails 5.2.3 here)*

<i class="fa fa-terminal"></i>`rails new MyApp --api && cd MyApp`

<i class="fa fa-terminal"></i>`rails g model product name:string sku:string:index 'price:decimal{10,2}'`
``` console
Running via Spring preloader in process 20474
      invoke  active_record
      create    db/migrate/20190624_create_products.rb
      create    app/models/product.rb
      invoke    test_unit
      create      test/models/product_test.rb
      create      test/fixtures/products.yml
```
<i class="fa fa-terminal"></i>`rails db:migrate`

Then we need some test data..
``` ruby
# test/fixtures/products.yml
# Read about fixtures at http://api.rubyonrails.org/classes/ActiveRecord/FixtureSet.html
one:
  name: Cheap Widget
  sku: CW123
  price: 9.99

two:
  name: Expensive Widget
  sku: EW456
  price: 99.99
```
..and of course a test..
``` ruby
# test/product_calculators/product_discount_calculator_test.rb
require 'test_helper'

class PriceCalculatorTest < ActiveSupport::TestCase
  test 'cheap widget is reduced by 15%' do
    widget = products(:one)
    calculated_price = PriceCalculator.get_price(widget)

    assert_equal((widget.price * 0.85), calculated_price, 'cheap widget should have a 15% price reduction')
  end
end
```
..which when run with the following command..

<i class="fa fa-terminal"></i>`rails test test/product_calculators/product_discount_calculator_test.rb`

..resulting in a `NameError: uninitialized constant`
``` console
E

Error:
PriceCalculatorTest#test_cheap_widget_is_reduced_by_15%:
NameError: uninitialized constant PriceCalculatorTest::PriceCalculator
    test/product_calculators/product_discount_calculator_test.rb:6:in `block in <class:PriceCalculatorTest>'
```
..as expected - our `PriceCalculator` doesn't exist yet and Rails is expecting the definition to be somewhere in `autoload_paths` and having not found a definition, fails with an error expecting the it to be within the calling code, in this case, within our test.

*Very helpful but not very organised.*

Let’s create a simple implementation and see if Rails can find it automatically:
``` ruby
# app/calculators/price_calculator.rb
# Simple calculator that takes a Product and returns a price with a 15% discount
class PriceCalculator
  def self.get_price(product)
    product.price * 0.85
  end
end
```
Now, when we re-run our rails test we get the same error - **our new class isn't found**. The reason is noted at the [end of section 5 in the aforementioned documention](https://guides.rubyonrails.org/autoloading_and_reloading_constants.html#autoload-paths-and-eager-load-paths):

> `autoload_paths` is computed and cached during the initialization process. The application needs to be restarted to reflect any changes in the directory structure.

We can check this is the reason by viewing the cached paths with the following command:

<i class="fa fa-terminal"></i>`bin/rails r 'puts ActiveSupport::Dependencies.autoload_paths'`
``` console
Running via Spring preloader in process 21407
/MyApp/app/channels
/MyApp/app/controllers
/MyApp/app/controllers/concerns
/MyApp/app/jobs
/MyApp/app/mailers
/MyApp/app/models
/MyApp/app/models/concerns
../ruby/gems/2.6.0/gems/activestorage-5.2.3/app/assets
../ruby/gems/2.6.0/gems/activestorage-5.2.3/app/controllers
../ruby/gems/2.6.0/gems/activestorage-5.2.3/app/controllers/concerns
../ruby/gems/2.6.0/gems/activestorage-5.2.3/app/javascript
../ruby/gems/2.6.0/gems/activestorage-5.2.3/app/jobs
../ruby/gems/2.6.0/gems/activestorage-5.2.3/app/models
/MyApp/test/mailers/previews
```
Yep, no mention of our new `app/calculators` directory. In development, [Spring](https://github.com/rails/spring) caches the directories so restarting our application after creating directories is akin to saying "restart Spring"..

<i class="fa fa-terminal"></i>`spring stop`

<i class="fa fa-terminal"></i>`bin/rails r 'puts ActiveSupport::Dependencies.autoload_paths'`
``` console
Running via Spring preloader in process 21462
/MyApp/app/calculators     <-- ** new directory **
/MyApp/app/channels
/MyApp/app/controllers
/MyApp/app/controllers/concerns
/MyApp/app/jobs
/MyApp/app/mailers
/MyApp/app/models
/MyApp/app/models/concerns
../ruby/gems/2.6.0/gems/activestorage-5.2.3/app/assets
../ruby/gems/2.6.0/gems/activestorage-5.2.3/app/controllers
../ruby/gems/2.6.0/gems/activestorage-5.2.3/app/controllers/concerns
../ruby/gems/2.6.0/gems/activestorage-5.2.3/app/javascript
../ruby/gems/2.6.0/gems/activestorage-5.2.3/app/jobs
../ruby/gems/2.6.0/gems/activestorage-5.2.3/app/models
/MyApp/test/mailers/previews
```
Our new directory is found, so if we re-run our test..

<i class="fa fa-terminal"></i>`rails test test/product_calculators/product_discount_calculator_test.rb`
``` console
# Running:

.

Finished in 0.028790s, 34.7343 runs/s, 34.7343 assertions/s.
```
Excellent. No `require` or `require_relative` required, and no change to `autoload_paths` or our application's configuration either.

## Adding namespaces
Organising our classes in sub-directories of `app/` is fine for smaller applications buy say we now wanted the following directory structure: `app/calculators/pricing/price_calculator.rb`

<i class="fa fa-terminal"></i>`mkdir app/calculators/pricing && mv app/calculators/*.rb app/calculators/pricing/`

<i class="fa fa-terminal"></i>`spring stop && rails test test/product_calculators/product_discount_calculator_test.rb`
``` console
# Running:

E

Error:
PriceCalculatorTest#test_cheap_widget_is_reduced_by_15%:
NameError: uninitialized constant PriceCalculatorTest::PriceCalculator
    test/product_calculators/product_discount_calculator_test.rb:6:in `block in <class:PriceCalculatorTest>'
```

If we inspect the list of `autoload_paths` again, we see our top-level `app/calculators` path so Rails needs a bit more of a hint to find our calculator..
We need to add a `Pricing::` namespace to consumers of our moved calculator class..
``` ruby
# test/product_calculators/product_discount_calculator_test.rb
require 'test_helper'

class PriceCalculatorTest < ActiveSupport::TestCase
  test 'cheap widget is reduced by 15%' do
    widget = products(:one)
    calculated_price = Pricing::PriceCalculator.get_price(widget)

    assert_equal((widget.price * 0.85), calculated_price, 'cheap widget should have a 15% price reduction')
  end
end
```
<i class="fa fa-terminal"></i>`rails test test/product_calculators/product_discount_calculator_test.rb`
``` console
# Running:

E

Error:
PriceCalculatorTest#test_cheap_widget_is_reduced_by_15%:
LoadError: Unable to autoload constant Pricing::PriceCalculator, expected /MyApp/app/calculators/pricing/price_calculator.rb to define it
    test/product_calculators/product_discount_calculator_test.rb:6:in `block in <class:PriceCalculatorTest>'
```
Rails is looking in the correct place, but now we need to put the class in a matching namespace..
``` ruby
# app/calculators/pricing/price_calculator.rb
# Simple calculator that takes a Product and returns a price with a 15% discount
module Pricing
  class PriceCalculator
    def self.get_price(product)
      product.price * 0.85
    end
  end
end
```
If we run our test again now it passes:

<i class="fa fa-terminal"></i>`rails test test/product_calculators/product_discount_calculator_test.rb`
``` console
# Running:

.

Finished in 0.023214s, 43.0775 runs/s, 43.0775 assertions/s.
1 runs, 1 assertions, 0 failures, 0 errors, 0 skips
```

Fine, but why don't we need to start the namespace with `Calculators::`? - well, this too is a Rails convention and explained in the documention:
> Rails has a collection of directories similar to `$LOAD_PATH` in which to look up `post.rb`. That collection is called `autoload_paths` and by default it contains:
>
> * All subdirectories of app in the application and engines present at boot time. For example, `app/controllers`. They do not need to be the default ones, any custom directories like `app/workers` belong automatically to `autoload_paths`.
> * ...

Therefore our custom `app/calculators` directory is automatically added.. as we saw earlier!

### How about 2 namespaces? (<i class="fa fa-fw fa-check"></i>)
Can we add 1 more namespace to our current `PriceCalculator` (2 namespaces / modules)?
``` ruby
# test/product_calculators/product_discount_calculator_test.rb
require 'test_helper'

class PriceCalculatorTest < ActiveSupport::TestCase
  test 'cheap widget is reduced by 15%' do
    widget = products(:one)
    calculated_price = Pricing::WidgetPricing::PriceCalculator.get_price(widget)

    assert_equal((widget.price * 0.85), calculated_price, 'cheap widget should have a 15% price reduction')
  end
end
```
``` ruby
# app/calculators/pricing/price_calculator.rb
# Simple calculator that takes a Product and returns a price with a 15% discount
module Pricing
  module WidgetPricing
    class PriceCalculator
      def self.get_price(product)
        product.price * 0.85
      end
    end
  end
end
```
then move it into a directory with the same namespace structure:

<i class="fa fa-terminal"></i>`mkdir app/calculators/pricing/widget_pricing && mv app/calculators/pricing/*.rb app/calculators/pricing/widget_pricing/`

test again..

<i class="fa fa-terminal"></i>`spring stop && rails test test/product_calculators/product_discount_calculator_test.rb`
and..
``` console
# Running:

.

Finished in 0.378896s, 2.6392 runs/s, 10.5570 assertions/s.
1 runs, 4 assertions, 0 failures, 0 errors, 0 skips
```
### 3 Namespaces? (<i class="fa fa-fw fa-check"></i>)
2 works, now let's try 3 namespaces. Same as above, first we alter the test, then the file, and then the file's location..
``` ruby
# test/product_calculators/product_discount_calculator_test.rb
require 'test_helper'

class PriceCalculatorTest < ActiveSupport::TestCase
  test 'cheap widget is reduced by 15%' do
    widget = products(:one)
    calculated_price = Pricing::WidgetPricing::TwoThousand::PriceCalculator.get_price(widget)

    assert_equal((widget.price * 0.85), calculated_price, 'cheap widget should have a 15% price reduction')
  end
end
```
``` ruby
# app/calculators/pricing/two_thousand/price_calculator.rb
# Simple calculator that takes a Product and returns a price with a 15% discount
module Pricing
  module WidgetPricing
    module TwoThousand
      class PriceCalculator
        def self.get_price(product)
          product.price * 0.85
        end
      end
    end
  end
end
```
<i class="fa fa-terminal"></i>`spring stop && rails test test/product_calculators/product_discount_calculator_test.rb`
``` console
# Running:

.

Finished in 0.047112s, 21.2260 runs/s, 21.2260 assertions/s.
1 runs, 1 assertions, 0 failures, 0 errors, 0 skips
```
### 4 Namespaces (<i class="fa fa-fw fa-check"></i>)
That also works - how about 4 namespaces?

``` ruby
# test/product_calculators/product_discount_calculator_test.rb
require 'test_helper'

class PriceCalculatorTest < ActiveSupport::TestCase
  test 'cheap widget is reduced by 15%' do
    widget = products(:one)
    calculated_price = Pricing::WidgetPricing::TwoThousand::Nineteen::PriceCalculator.get_price(widget)

    assert_equal((widget.price * 0.85), calculated_price, 'cheap widget should have a 15% price reduction')
  end
end
```
``` ruby
# app/calculators/pricing/two_thousand/nineteen/price_calculator.rb
# Simple calculator that takes a Product and returns a price with a 15% discount
module Pricing
  module WidgetPricing
    module TwoThousand
      module Nineteen
        class PriceCalculator
          def self.get_price(product)
            product.price * 0.85
          end
        end
      end
    end
  end
end
```
.. and it still works - all without touching the Rails app configuration files.

### 5 Namespaces (<i class="fa fa-fw fa-check"></i>)
Hmm.. so what happens if we move our `PricingCalculator` from `app/calculators/...` to `app/lib/...`:

<i class="fa fa-terminal"></i>`spring stop && rails test test/product_calculators/product_discount_calculator_test.rb`
``` console
# Running:

.

Finished in 0.052509s, 19.0444 runs/s, 19.0444 assertions/s.
1 runs, 1 assertions, 0 failures, 0 errors, 0 skips
```
And if we were to add a `calculators` sub-directory under `app/lib/` like so:
``` ruby
# app/lib/calculators/pricing/widget_pricing/tow-thousand/nineteen/price_calculator.rb
# Simple calculator that takes a Product and returns a price with a 15% discount
module Calculators
  module Pricing
    module WidgetPricing
      module TwoThousand
        module Nineteen
          class PriceCalculator
            def self.get_price(product)
              product.price * 0.85
            end
          end
        end
      end
    end
  end
end
```
Why would we do this? Well, our use of this class within our code (and our tests) would be a little more self-documenting:
``` ruby
# test/product_calculators/product_discount_calculator_test.rb
require 'test_helper'

class PriceCalculatorTest < ActiveSupport::TestCase
  test 'cheap widget is reduced by 15%' do
    widget = products(:one)
    calculated_price = Calculators::Pricing::WidgetPricing::TwoThousand::Nineteen::PriceCalculator.get_price(widget)

    assert_equal((widget.price * 0.85), calculated_price, 'cheap widget should have a 15% price reduction')
  end
end
```
That's clearer, if a little exaggerated for the purposes of experimentation and illustration 🙂


## Summary
Rails convention over configuration lets us organise our classes into nested namespace / module hierarchies as long as they match the directory or folder structure, starting one subdirectory in from `app/` - e.g. `app/lib`.

<div class="panel panel-warning">
  <div class="panel-body bg-warning">
    <i class="fa fa-fw fa-sticky-note"></i><strong>There are 3 things to remember:</strong>
    <ol>
      <li>Our application's code should live in <code>/app/..</code> - personally I recommend <code>/app/lib/{sub_directory}/..</code> so we can use <code>{sub_directory}</code> as the start of our namespacing: e.g. <code>app/lib/calculators/... -> Calculators::...</code></li>
      <li>When moving files around or creating new directories we need to restart <code>Spring</code> (<i class="fa fa-terminal"></i><code>spring stop</code>)</li>
      <li>To reference a class that isn't in a namespace (or in the root namespace), such as a Rails Model in its default location of <code>app/models</code>, we can use the namespace separator (<code>::</code>) as a prefix - e.g.: <code>::Product</code></li> - this is useful if we also have a class of the same name in a different namespace.
    </ol>
  </div>
</div>

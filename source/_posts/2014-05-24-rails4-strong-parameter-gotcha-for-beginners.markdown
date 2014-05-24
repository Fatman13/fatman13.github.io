---
layout: post
title: "Rails4 strong parameter gotcha for beginners"
date: 2014-05-24 21:24
comments: true
categories: 
- 技术
- rails
---
`Rails 4` gotcha for noobies(like me) -- Strong Parameters

<!--more-->

`Rails 4` now comes with [strong_parameters](https://github.com/rails/strong_parameters) gem as part of standard installation now. For example if you run `rails g scaffold product name price:decimal`, the following will be generated in `app/controllers/products_controller.rb` under `private` methods section.

``` ruby
# Never trust parameters from the scary internet, only allow the white list through.
def product_params
  params.require(:product).permit(:name, :price)
end
```

As the comments suggested, `product_params` will only allow a whitelist of product attributes. This is done by using `permit` method as shown above. 

Now the gotcha part... When you add new columns using `rails g migration add_stock_to_product stock:integer`, `rake db:migrate`, changing the corresponding scaffold `html.erb` files. You expect the new product attribute `stock` to work, but it won't. You still need to add `:stock` hash to `permit` method like the following.


``` ruby
# Never trust parameters from the scary internet, only allow the white list through.
def product_params
  params.require(:product).permit(:name, :price, :stock)
end
```

I kept forgetting adding new attribute to `permit`. Thus render controller methods not recognizing new attributes. It got me twice, so putting a note here. Hopefully helping someone on the way.

### Garner
---
https://github.com/artsy/garner

```
get "/system/counts/all" do
  garner do
    {
      "orders_count" => Order.count,
      "users_count" => User.count
    }
  end
end

get "/me/address" do
  garner.bind(current_user) do
    current_user.address
  end
end

require "garner/mixins/mongoid"
module Mongoid
  module Document
    include Garner::Mixins::Mongoid::Document
  end
end

get "/system/counts/orders" do
  garner.bind(Order) do
    {
      "orders_count" => Order.count,
    }
  end
end

get "/order/:id" do
  garner.bind(Order.identify(params:[:id])) do
    Order.find(params[:id])
  end
end

Garner.configure do |config|
  config.mongoid_identity_fields = [:_id, :_slugs]
end

order = Order.garnered_find(3)

require "garner/mixins/active_record"
module ActiveRecord
  class Base
    include Garner::Mixins::ActiveRecord::Base
  end
end

get "/latest_order" do
  garner.options(expires_in: 15.minutes) do
    Order.latest
  end
end

get "/system/counts/all" do
  garner.bind(Order).bind(User) do
    {
      "orders_count" => Order.count,
      "users_count" => User.count
    }
  end
end

get "/order/:id" do
  garner.bind(Order.identity(params[:id])).key({ role: current_usr.role }) do
    Order.find(params[:id])
  end
end

Garner.configure do |config|
  config.cache = ActiveSupport::Cache::FileStore.new
end

```

```
[
  Garner::Strategies::Context::Key::Caller,
  Garner::Strategies::Context::Key::RequestGet,
  Garner::Strategies::Context::Key::RequestPost,
  Garner::Strategies::Context::Key::RequestPath
]
```

```
```

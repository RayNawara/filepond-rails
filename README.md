# filepond-rails

This gem allows you to quickly integrate [FilePond](https://github.com/pqina/filepond) with your Ruby on Rails app.

## Requirements
* Rails 7.0.x and above
* Use of [importmap-rails](https://github.com/rails/importmap-rails) (optional)

## Installation
Add this line to your application's Gemfile:

```ruby
gem 'filepond-rails'
```

And then run:
```bash
$ bundle
```

Add the following to your `application.css`:
```css
*= require 'filepond'
```

Add `javascript_importmap_tags` in the `head` section of your layout:
```erb
<%= javascript_importmap_tags %>
```

Mount `filepond-rails` routes:
```ruby
Rails.application.routes.draw do
  mount Filepond::Rails::Engine, at: '/filepond'
end
```

Note: You must mount the routes at `/filepond`.

## Usage

This gem compromises of two parts:
1. An ingress controller for the FilePond `fetch` and `remove` server calls. The JavaScript code leverages Active Storage's existing direct upload endpoint.
2. JavaScript ESM module to automatically connect configured elements with the above endpoints.

At the present moment, the above parts are designed (and assumed) to work together.

The gem assumes that you set up your models and Active Storage configuration in a standard Rails way. Assuming you have a form that looks like this:

```erb
<%= form_with model: @user, url: update_avatar_path, method: :post do |f| %>
  <%= f.label :avatar, 'Update your avatar' %>
  <%= f.file_field :avatar, class: 'filepond', direct_upload: true %>
  <%= f.button 'Update' %>
<% end %>
```

You can instantiate the `file_field` component like so:

```js
// app/javascript/application.js
import { FilePondRails, FilePond } from 'filepond-rails'

window.FilePond = FilePond
window.FilePondRails = FilePondRails

const input = document.querySelector('.filepond')
FilePondRails.create(input)
```

The gem's JavaScript provide two available exports:
1. `FilePond` (which corresponds to the original FilePond library)
2. `FilePondRails` (which is a convenience helper that integrates and sets up FilePond to work with our `FilePond::Rails::IngressController`

## License
The gem is available as open source under the terms of the [MIT License](https://opensource.org/licenses/MIT).

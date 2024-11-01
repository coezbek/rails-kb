# rails-kb
My personal knowledge base for Rails development

## CSS handling in Rails 7.1

Moving from older version of Rails to Rails 7.1, I found it difficult to wrap my head around how CSS should be handled best in Rails 7.1 and how to solve typical situations.

What is the default if you just run `rails new $appname`?

- In Rails 7.1 the default for JS is now `importmaps`, which avoids using any bundler (`webpack` or `node`-based bundlers such as `esbuild`).
- In Rails 7.1 the default for CSS is now plain `sprockets`, which means you don't have support for `scss`/`sass`.

What options exist for css bundling in Rails 7.1?

- There are three options when generating a new Rails 7.1 app: [Tailwind CSS standalone compiler](https://github.com/rails/tailwindcss-rails) (via `tailwindcss-rails` gem), [Dart Sass standalone compiler](https://github.com/rails/dartsass-rails) (via `dartsass-rails` gem) and [CSS Bundling via Node.js]() (via `cssbundling-rails` gem). While the first two are gems which wrap the standalone CLI tools for Tailwind and Saas bundling, the `cssbundling-rails` gem uses one of the following Javascript packages to perform the bundling via `node`: `tailwind|bootstrap|bulma|postcss|sass`
   - Note: Using `cssbundling-rails` with option `tailwind` or `saas` is not the same as using the `tailwindcss-rails` and `dartsaas-rails` gem. The former uses Node with the JS package, the latter uses the standalone command line tools.

## Dealing with enums / boolean in _form and _partial

I always struggle to remember the options for collection_radio_group et al.

Unfortunately there is no native support for enums in the Rails generators (see https://github.com/rails/rails/issues/21970). 

Things I learned / my best practices:
 * In Controller/Views, Rails treats enums purely as text. All methods return and receive the text of the enum options. The integer option used for storing the result is just used towards the database.
 * Enums are defined as positional arguments: `enum :enum_name, { enum_option1: 0, enum_option2: 1 }`

```
    <!-- Enum field, Options shown inline in PicoCSS -->
    <fieldset>
      <legend>Gender enum</legend>
      <%= 
        form.collection_radio_buttons(:gender, User.genders, :first, :first) do |b| 
          b.radio_button + b.label { b.text.humanize }
        end 
      %>
    </fieldset>

    <!-- Boolean field as two Radio Buttons, Options shown inline in PicoCSS -->
    <fieldset>
      <%= form.label(:visible_to_others) do |builder| %>
        <%= builder.translation %>
      <% end %>      
      <%= form.radio_button :visible_to_others, true %> <%= form.label :visible_to_others, "Visible to other users", value: true %>
      <%= form.radio_button :visible_to_others, false %> <%= form.label :visible_to_others, "Hidden from other users", value: false %>
    </fieldset>
```

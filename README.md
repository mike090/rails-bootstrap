# README

adapted from [BootrAils](https://www.bootrails.com/blog/rails-bootstrap-tutorial/)

Ruby version 3.0.2

1. Create simple Rails application with test page
    ```bash
    rails _6.1.4.4_ new rails-bootstrap && cd rails-bootstrap
    ```
    * add test page
        ```bash
        rails g controller welcome index
        ```
        *app/views/welcome/index.html.erb*
        ```html
        <div class="collapse" id="navbarToggleExternalContent">
        <div class="bg-dark p-4">
            <h5 class="text-white h4">Collapsed content</h5>
            <span class="text-muted">Toggleable via the navbar brand.</span>
        </div>
        </div>
        <nav class="navbar navbar-dark bg-dark">
        <div class="container-fluid">
            <button class="navbar-toggler" type="button" data-bs-toggle="collapse" data-bs-target="#navbarToggleExternalContent" aria-controls="navbarToggleExternalContent" aria-expanded="false" aria-label="Toggle navigation">
            <span class="navbar-toggler-icon"></span>
            </button>
        </div>
        </nav>

        <main>
        <section class="py-5 text-center container">
            <div class="row py-lg-5">
            <div class="col-lg-6 col-md-8 mx-auto">
                <h1 class="fw-light">Album example</h1>
                <p class="lead text-muted">Something short and leading about the collection below—its contents, the creator, etc. Make it short and sweet, but not too short so folks don’t simply skip over it entirely.</p>
                <p>
                <a href="#" class="btn btn-primary my-2">Main call to action</a>
                <a href="#" class="btn btn-secondary my-2">Secondary action</a>
                </p>
            </div>
            </div>
        </section>
        </main>

        ```
    * config routes
        *config/routes.rb*
        ```ruby
        ...
        root 'welcome#index'
        get 'welcome/index'
        ...
        ```
    * ensure the application works
        ```bash
        bin/rails server
        ```

1. Add Bootstrap to application
    * make sure webpacker included in Gemfile
        ```ruby
        # Transpile app-like JavaScript. Read more: https://github.com/rails/webpacker
        gem 'webpacker', '~> 5.0'
        ```
    * handle all our assets, not just JavaScript to webpacker:
        ```bash
        git mv app/javascript app/frontend
        git mv app/frontend/packs app/frontend/entrypoints
        ```
    * change assets folder name

 	    _config/webpacker.yml_
        ```ruby
        default: &default 
            source_path: app/frontend # change here
            source_entry_path: entrypoints # and here
        ```
    * install Bootstrap and dependency
        ```bash
        yarn add bootstrap
        yarn add @popperjs/core
        ```
    * inject Bootstrap into your app
        * import Bootstrap-related JS files
            ```
            mkdir app/frontend/js && touch app/frontend/js/bootstrap_js_files.js
            ```

            *app/frontend/js/bootstrap_js_files.js*
            ```javascript
            // import 'bootstrap/js/src/alert'
            // import 'bootstrap/js/src/button'
            // import 'bootstrap/js/src/carousel'
            import 'bootstrap/js/src/collapse'
            import 'bootstrap/js/src/dropdown'
            // import 'bootstrap/js/src/modal'
            // import 'bootstrap/js/src/popover'
            import 'bootstrap/js/src/scrollspy'
            // import 'bootstrap/js/src/tab'
            // import 'bootstrap/js/src/toast'
            // import 'bootstrap/js/src/tooltip'
            ```

            *app/frontend/packs/application.js*
            ```javascript
            import '../js/bootstrap_js_files.js'
            ```

        * inject bootstrap styles
            ```bash
            mkdir app/frontend/stylesheets && touch app/frontend/stylesheets/application.scss
            ```
            *app/frontend/stylesheets/application.scss*
            ```css
            /*Just a quick ugly style to see if our CSS works*/
            h1 {
                text-decoration: underline;
            }
            /*Import Bootstrap v5*/
            @import "~bootstrap/scss/bootstrap";
            ```
        * and impot it in
            *app/frontend/packs/application.js*
            ```javascript
            import '../stylesheets/application'
            ```


        * add main css file to layout
            *app/views/layouts/application.html.erb*
            ```html
            <head>
                <title>Myapp</title>
                <meta name="viewport" content="width=device-width,initial-scale=1">
                <%= csrf_meta_tags %>
                <%= csp_meta_tag %>

                <!-- Warning !! ensure that "stylesheet_pack_tag" is used, line below -->
                <%= stylesheet_pack_tag 'application', media: 'all', 'data-turbolinks-track': 'reload' %>
                <%= javascript_pack_tag 'application', 'data-turbolinks-track': 'reload' %>
            </head>
            ```
        * compile assets
            ```bash
            bin/rails webpacker:compile
            ```
        * make sure everything works
            ```bash
            bin/rails server
            ````

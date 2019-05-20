# Rails Kata 001

1.

```bash
rails g model Post title:string body:text
```

```bash
rake db:migrate
```

2. /config/routes.rb

```ruby
resources :posts
```

3.

```bash
rails g controller Posts
```

4. /app/controllers/post_controllers.rb

```ruby
class PostController < ApplicationController
  before_action :set_post, only: [:show, :edit, :update, :destroy]

  def index
    @posts = Post.all
  end

  def show
  end

  def new
    @post = Post.new
  end

  def edit
  end

  def create
    @post = Post.new(post_params)
    if @post.valid?
      @post.save
      redirect_to @post
    else
      render :new
    end
  end

  def update
    if @post.update(post_params)
      redirect_to @post
    else
      render :edit
    end
  end

  def destroy
    @post.destroy
    redirect_to posts_path
  end

  private

  def set_post
    @post = Post.find(params[:id])
  end

  def post_params
    params.require(:post).permit(:title, :body)
  end
end
```

5.

```bash
cd app/views/posts/
touch index.html.erb show.html.erb new.html.erb edit.html.erb _post.html.erb _form.html.erb
```

6. index.html.erb

```ruby
<h1>Posts</h1>

<%= 'No posts' if @posts.empty? %>

<%= render @posts %>
```

7. _post.html.erb

```ruby
<h3><%= link_to post.title, post_path(post) %></h3>
```

8. show.html.erb

```ruby
<h1><%= @post.title %></h1>

<p><%= @post.body %></p>

<p>
  <%= link_to 'Edit post', edit_post_path(@post) %> |
  <%= link_to 'Delete post', post_path(@post), method: :delete, data: { confirm: 'Really?' } %>
</p>
```

9. new.html.erb

```ruby
<h1>New post</h1>

<%= render 'form' %>
```

10. edit.html.erb

```ruby
<h1>Edit post</h1>

<%= render 'form' %>
```

11. _form.html.erb

```ruby
<%= form_for @post do |f| %>
  <p>
    <%= f.label :title %>
    <%= f.text_field :title %>
  </p>

  <p>
    <%= f.label :body %>
    <%= f.text_area :body %>
  </p>

  <p>
    <%= f.submit 'Save' %> |
    <%= link_to 'Cancel', posts_path %>
  </p>

<% end %>
```

12. app/models/post.rb

```ruby
class Post < ApplicationRecord
  validates :title, :body, presence: true
end
```

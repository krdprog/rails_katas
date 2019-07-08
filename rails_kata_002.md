# Rails Kata 002

## Добавление комментария к посту (продолжение Rails Kata 001)

1.

```bash
rails g model Comment author:string body:text post:references
```

```bash
rake db:migrate
```

2. /models/project.rb

```ruby
has_many :comments, dependent: :destroy
```

3. /models/comment.rb

```ruby
belongs_to :posts
```

4. /config/routes.rb

```ruby
resources :posts do
  resources :comments, only: [:create, :destroy]
end
```

5.

```bash
rails g controller Comments
```

6. /comments_controller.rb

```ruby
def create
  @post = Post.find(params[:post_id])
  @post.comments.create(comment_params)

  redirect_to post_path(@post)
end

def destroy
  @post = Post.find(params[:post_id])
  @post.comments.find(params[:id]).destroy

  redirect_to post_path(@post)
end

private

def comment_params
  params.require(:comment).permit(:author, :body)
end
```

7. show.html.erb for post

```ruby
<h3>Comments:</h3>

<% @post.comments.each do |comment| %>
  <h4>Author: <%= comment.author %></h4>
  <p>
  <% link_to 'Delete comment', post_comment_path(@post, comment), method: delete, data: { confirm: 'Really?' } %>
  </p>

  <p><%= comment.body %></p>
<% end %>

<% form_for([@post, @post.comments.build]) do |f| %>
  <p>
    <%= f.label :author %>
    <%= f.text_field :author %>
  </p>

  <p>
    <%= f.label :body %>
    <%= f.text_area :body %>
  </p>

  <p><%= f.submit 'Send' %></p>
<% end %>
```
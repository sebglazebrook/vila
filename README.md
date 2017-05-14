# Vila

Vila help your create REST services in Ruby easily.


## Installation

Install the gem locally:

```sh
gem install 'vila'
```

And then execute:

```sh
vila new <service_name>
```

Now look inside the project that was created for you:

```sh
cd <service_name> && ls -la
```

## Usage

Using Vila is pretty easy, basically you just hook up urls to resources using the router.

Here's an example:

```ruby
# config/router.rb
class Router < Vila::Router

  resource :user

end
```


Now you need to create a resource inside the project's resources dir:

```sh
touch resources/user.rb
```

Now you need a method to handle each REST request type:

```ruby
# resources/user.rb
class User < Vila::Resource

  def get(params)
  end

  def get_with_id(id, params)
  end

  def post(params)
  end

  def put(id, params)
  end

  def patch(id, params)
  end

  def delete(id, params)
  end

  def head(params)
  end

  def options(params)
  end

end
```

Sweet, now each of these methods can return either the body of the response:

 - Either a string or something that responds to `to_s`
 - or a hash containing a code: body: headers: keys/values

Example:

```ruby
  def get(params)

    # Below we return the body of the reponse
    # it will get a default http status code of 200 as it's a get request

    UserRepo.all.to_json

  end

  def post(params)

    # Below we return a hash containing the body of the response and a response code
    # this get's merged into the default hash which includes any headers we are returning
    # if we didn't include a response code it would default to 201 as this is a post which generally
    # creates resources

    user = UserFactory.create(params[:user])
    { code: 201, body: user.to_json }

  end
```

Now we can start the app:

```sh
vila server
```

Any then just hit your resource to test it:

```sh
curl localhost:6969/users/
```

## Development

After checking out the repo, run `bin/setup` to install dependencies. Then, run `rake spec` to run the tests. You can also run `bin/console` for an interactive prompt that will allow you to experiment.

To install this gem onto your local machine, run `bundle exec rake install`. To release a new version, update the version number in `version.rb`, and then run `bundle exec rake release`, which will create a git tag for the version, push git commits and tags, and push the `.gem` file to [rubygems.org](https://rubygems.org).

## Contributing

Bug reports and pull requests are welcome on GitHub at https://github.com/[USERNAME]/vila.


## License

The gem is available as open source under the terms of the [MIT License](http://opensource.org/licenses/MIT).


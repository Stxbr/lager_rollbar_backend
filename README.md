# Lager Backend for Rollbar

This is a [lager][lager] backend for the error notification service [Rollbar][rollbar].
The backend mediates between lager and [rollbar][rollbar]. It filters out all messages except crash reports and lager:[error|notice|warning|debug] messages.

## Usage

You need an [Rollbar][rollbar] account. Your application should be OTP conformant and use `rebar`.

Add `lager_rollbar_backend` to the dependencies in your `rebar.config`:

    {lager_rollbar_backend, ".*", {git, "https://github.com/lrascao/lager_rollbar_backend.git", {tag, "0.0.2"}}}

Include `lager_rollbar_backend` in the `lager` configuration of your project:

    {lager, [
        {handlers, [
            {lager_rollbar_backend, [
                {access_token, "ACCESS_TOKEN"},
                {environment, development},
                {platform, mobile},
                {level, error},  %% optional, default is error
                %% the following are optional
                {transport, {my_module, my_send_method}}]}]}]}.


The backend will send log messages with log level `error` and more critical to Rollbar.
The parameters are as follows:

   * [access_token][access_token] - the `post_server_item` token obtained from the project's Rollbar account
   * [environment][environment] - project environment (eg. development, production, etc)
   * [platform][platform] - project platform
   * [level][level] - level from which to log from
   * [transport][transport] - tuple containing Module:Method to be invoked upon data ready, default is lager_rollbar_backend:http_client/1, takes binary data and sends it via http to https://api.rollbar.com/api/1/item

### Build

You can build `lager_rollbar_backend` on its own with:

    make deps compile

### Dependencies

Dependencies are listed here for completeness, but are managed with `rebar`:

* [lager][lager]
* [rollbar][rollbar]

[lager]: <http://github.com/basho/lager> "lager"
[erollbar]: <http://github.com/lrascao/rollbar> "rollbar"
[rollbar]: <http://rollbar.com> "Rollbar"

## Authors

- Luis Rascão (lrascao) <luis.rascao@gmail.com>

## License

lager_rollbar_backend is available under the MIT license (see `LICENSE`).

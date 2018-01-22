# Puppet::ResourceApi [![TravisCI Build Status](https://travis-ci.org/puppetlabs/puppet-resource_api.svg?branch=master)](https://travis-ci.org/puppetlabs/puppet-resource_api) [![Appveyor Build status](https://ci.appveyor.com/api/projects/status/8o9s1ax0hs8lm5fd/branch/master?svg=true)](https://ci.appveyor.com/project/puppetlabs/puppet-resource-api/branch/master)
 [![codecov](https://codecov.io/gh/puppetlabs/puppet-resource_api/branch/master/graph/badge.svg)](https://codecov.io/gh/puppetlabs/puppet-resource_api)


This is an implementation of the [Resource API](https://github.com/DavidS/puppet-specifications/blob/resourceapi/language/resource-api/README.md) proposal. Find a working example of a new-style provider in the [experimental puppetlabs-apt branch](https://github.com/DavidS/puppetlabs-apt/blob/resource-api-experiments/lib/puppet/provider/apt_key2/apt_key2.rb). There is also the corresponding [type](https://github.com/DavidS/puppetlabs-apt/blob/resource-api-experiments/lib/puppet/type/apt_key2.rb), [provider](https://github.com/DavidS/puppetlabs-apt/blob/resource-api-experiments/lib/puppet/provider/apt_key2/apt_key2.rb), and [new unit tests](https://github.com/DavidS/puppetlabs-apt/blob/resource-api-experiments/spec/unit/puppet/provider/apt_key2/apt_key2_spec.rb) for 100% coverage.

## Getting started

* Install the [PDK](https://puppet.com/download-puppet-development-kit).
  * As of Jan 2018, the required PDK features are still in development.
    See https://github.com/puppetlabs/pdk-templates/pull/13 and https://github.com/puppetlabs/pdk/pull/409 for progress.

* Create a [new module](https://puppet.com/docs/pdk/latest/pdk_generating_modules.html) with the PDK, or work with an existing PDK-enabled module.

* Add the `puppet-resource_api` gem, and enable "modern" rspec-style mocking through the `.sync.yml`:

```
# .sync.yml
---
Gemfile:
  optional:
    ':development':
      - gem: 'puppet-resource_api'
spec/spec_helper.rb:
  mock_with: ':rspec'
```

*  Apply the changes by running `pdk convert`:

```
~/git/example$ ~/git/pdk/bin/pdk convert

----------Files to be modified----------
Gemfile
spec/spec_helper.rb

----------------------------------------

You can find a report of differences in convert_report.txt.

pdk (INFO): Module conversion is a potentially destructive action. Ensure that you have committed your module to a version control system or have a backup, and review the changes above before continuing.
Do you want to continue and make these changes to your module? Yes
[✔] Resolving Gemfile dependencies.

------------Convert completed-----------

2 files modified.

~/git/example$
```

* Create the required files for a new type and provider in the module with `pdk new provider <provider_name>`.

```
~/git/example$ pdk new provider foo
pdk (INFO): Creating '/home/david/git/example/lib/puppet/type/foo.rb' from template.
pdk (INFO): Creating '/home/david/git/example/lib/puppet/provider/foo/foo.rb' from template.
pdk (INFO): Creating '/home/david/git/example/spec/unit/puppet/provider/foo/foo_spec.rb' from template.
~/git/example$
```

The three generated files are the type, the actual implementation, and unit tests. The default template contains a trivial example, to demonstrate the basic workings of the Resource API. This allows the unit tests to run immediately after creating the provider:

```
~/git/example$ pdk test unit
[✔] Installing missing Gemfile dependencies.
[✔] Preparing to run the unit tests.
[✔] Running unit tests.
  Evaluated 5 tests in 0.018781355 seconds: 0 failures, 0 pending.
[✔] Cleaning up after running unit tests.
~/git/example$
```

See The [Resource API](https://github.com/DavidS/puppet-specifications/blob/resourceapi/language/resource-api/README.md) for the targeted capabilities of this gem.


## Known Issues

This gem is still under heavy development. This section is a living document of what's already done, and what items are still outstanding.

Already working:
* basic type and provider definition, using `name`, `desc`, and `attributes`
* the `canonicalize` and `remote_resource` features
* all the logging facilities
* executing the new provider under any of the following commands:
  * `puppet apply`
  * `puppet resource`
  * `puppet agent`
  * `puppet device` (if applicable)


There are still a few notable gaps between the implementation, and the specification:
* Only a single runtime environment (the puppet commands) is currently implemented.
* `auto*` definitions
* the Commands API is mostly implemented, but deployment is blocked on upstream work (PDK-580). You can use regular Ruby `system()` calls as a workaround, with all their underlying encoding, and safety issues.

## Development

After checking out the repo, run `bin/setup` to install dependencies. Then, run `rake spec` to run the tests. You can also run `bin/console` for an interactive prompt that will allow you to experiment.

To install this gem onto your local machine, run `bundle exec rake install`. To release a new version, update the version number in `version.rb`, and then run `bundle exec rake release`, which will create a git tag for the version, push git commits and tags, and push the `.gem` file to [rubygems.org](https://rubygems.org).

## Contributing

Bug reports and pull requests are welcome on GitHub at https://github.com/[USERNAME]/puppet-resource_api.

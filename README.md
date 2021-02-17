# Hipo CI for iOS Initial Directory

This repo contains Hipo's fastlane files for new iOS projects. In this documentation you can find how to use it and how to customize it.

## Installation

See the steps below for details about the installation.

1. [Download Files](README.md#setup-dependencies)
1. [Setup Dependencies](README.md#setup-dependencies)
1. [Add project to Bitrise](README.md#add-project-to-bitrise)
1. [Customization](README.md#customization)


### Download Files
You can directly download the files [here](https://github.com/Hipo/ci-integration-ios/archive/main.zip) and copy items to project folder. -except Fastfile-

### Setup Dependencies

We prefer installing fastlane via *Bundler*. It also helps to track dependencies easily. So you can see *Gemfile* in the project.

* Run `$ bundle update`

It will install fastlane and other dependencies that we use to your project. **Make sure** that all files added to version control.  

### Add project to Bitrise

After you complete the installation, you need to setup Bitrise for that project. It will automatically understand the project contains fastlane integration.


## Customization

If project needs some special CI processes, you may add them to your Fastfile. For example;

```ruby
platform :ios do
  lane :print_app_name do
    puts ENV["APP_NAME"]
  end
end
```

It prints the `APP_NAME` environment variable. And you can use public lanes as well in this lane.

Also you can override the public lanes if **needed**

```ruby
platform :ios do
  override_lane :process_variables do
    remove_unused_variables
  end
end
```

It does override the remote lane - `process_variables` and call another public lane or project specific lane. 

In general, you won't need customization but it can be done easily whenever it's necessary.
fastlane_version "2.185.0"

default_platform(:ios)

platform :ios do

   env_variables = {
    core: {
      fastlane_user: "FASTLANE_USER",
      team_id: "TEAM_ID",
      app_name: "APP_NAME",
      slack_webhook_url: "SLACK_WEBHOOK_URL",
      upload_symbols_path: "UPLOAD_SYMBOLS_PATH",
      #Store
      app_id: "APP_ID",
      scheme: "SCHEME",
      ipa_name: "IPA_NAME",
      info_plist: "GOOGLE_SERVICE_INFO_PLIST_PATH",
      app_store_build_config: "APP_STORE_BUILD_CONFIGURATION",
      itc_name: "FASTLANE_ITC_TEAM_NAME",
      #Prod
      prod_scheme: "PROD_SCHEME",
      prod_ipa_name: "PROD_IPA_NAME",
      prod_info_plist: "PROD_GOOGLE_SERVICE_INFO_PLIST_PATH",
      adhoc_build_config: "ADHOC_BUILD_CONFIGURATION",
      #Preprod
      preprod_app_id: "PREPROD_APP_ID", 
      preprod_scheme: "PREPROD_SCHEME",
      preprod_ipa_name: "PREPROD_IPA_NAME",
      preprod_info_plist: "PREPROD_GOOGLE_SERVICE_INFO_PLIST_PATH",
      #Staging
      staging_app_id: "STAGING_APP_ID", 
      staging_scheme: "STAGING_SCHEME",
      staging_ipa_name: "STAGING_IPA_NAME",
      staging_info_plist: "STAGING_GOOGLE_SERVICE_INFO_PLIST_PATH"
    },

    s3: {
      bucket: "S3_BUCKET",
      access_key: "S3_ACCESS_KEY",
      secret_access_key: "S3_SECRET_ACCESS_KEY",
      region: "S3_REGION"
    },

    match: {
      key_id: "KEY_ID",
      issuer_id: "ISSUER_ID",
      key_content: "KEY_CONTENT",
      in_house: "IN_HOUSE",
      match_password: "MATCH_PASSWORD",
    }
  }

  required_env_variables = {
    shared: [
      env_variables[:core][:fastlane_user],
      env_variables[:core][:team_id],
      env_variables[:core][:app_name],
      env_variables[:core][:slack_webhook_url],
      env_variables[:s3][:bucket],
      env_variables[:s3][:access_key],
      env_variables[:s3][:secret_access_key],
      env_variables[:s3][:region],
      env_variables[:match][:key_id],
      env_variables[:match][:issuer_id],
      env_variables[:match][:key_content],
      env_variables[:match][:in_house],
      env_variables[:match][:match_password]
    ],

    store: [
      env_variables[:core][:app_id],
      env_variables[:core][:scheme],
      env_variables[:core][:ipa_name],
      env_variables[:core][:info_plist],
      env_variables[:core][:app_store_build_config],
      env_variables[:core][:itc_name]
    ],

    prod: [
      env_variables[:core][:app_id],
      env_variables[:core][:prod_scheme],
      env_variables[:core][:prod_ipa_name],
      env_variables[:core][:prod_info_plist],
      env_variables[:core][:adhoc_build_config]
    ],

    preprod: [
      env_variables[:core][:preprod_app_id],
      env_variables[:core][:preprod_scheme],
      env_variables[:core][:preprod_ipa_name],
      env_variables[:core][:preprod_info_plist],
      env_variables[:core][:adhoc_build_config]
    ],

    staging: [
      env_variables[:core][:staging_app_id],
      env_variables[:core][:staging_scheme],
      env_variables[:core][:staging_ipa_name],
      env_variables[:core][:staging_info_plist],
      env_variables[:core][:adhoc_build_config]
    ]
  }

  before_all do |lane, options|
    process_variables

    ensure_env_vars(
      env_vars: required_env_variables[:shared]
    )

    if ENV[env_variables[:core][:app_id]] != nil
      ensure_env_vars(
        env_vars: required_env_variables[:store]
      )

      ensure_env_vars(
        env_vars: required_env_variables[:prod]
      )
    end

    if ENV[env_variables[:core][:preprod_app_id]] != nil
      ensure_env_vars(
        env_vars: required_env_variables[:preprod]
      )
    end

    if ENV[env_variables[:core][:staging_app_id]] != nil
      ensure_env_vars(
        env_vars: required_env_variables[:staging]
      )
    end
  end

  lane :process_variables do
  end

  # PUBLIC LANES

  lane :build_store_app do |options|
    build(
      app_identifier: ENV[env_variables[:core][:app_id]],
      scheme: ENV[env_variables[:core][:scheme]],
      is_store: true
    )
  end

  lane :build_staging_app do |options|
    build(
      app_identifier: ENV[env_variables[:core][:staging_app_id]],
      scheme: ENV[env_variables[:core][:staging_scheme]],
      is_store: false
    )
  end

  lane :build_preprod_app do |options|
    build(
      app_identifier: ENV[env_variables[:core][:preprod_app_id]],
      scheme: ENV[env_variables[:core][:preprod_scheme]],
      is_store: false
    )
  end

  lane :build_prod_app do |options|
    build(
      app_identifier: ENV[env_variables[:core][:app_id]],
      scheme: ENV[env_variables[:core][:prod_scheme]],
      is_store: false
    )
  end

  private_lane :app_name_for_target do |options|
    app_name = "#{ENV[env_variables[:core][:app_name]]} #{options[:target]}"
    app_name
  end

  lane :deploy_all_apps_to_testflight do |options|
    deploy_staging_app_to_testflight
    deploy_preprod_app_to_testflight
    deploy_store_app_to_testflight
  end

  lane :deploy_staging_app_to_testflight do |options|
    deploy_to_testflight(
      target: "Staging",
      app_identifier: ENV[env_variables[:core][:staging_app_id]],
      scheme: ENV[env_variables[:core][:staging_scheme]],
      ipa_name: ENV[env_variables[:core][:staging_ipa_name]],
      export_method: "ad-hoc",
      google_service_info_plist_path: ENV[env_variables[:core][:staging_info_plist]]
    )
  end

  lane :deploy_preprod_app_to_testflight do |options|
    deploy_to_testflight(
      target: "Preprod",
      app_identifier: ENV[env_variables[:core][:preprod_app_id]],
      scheme: ENV[env_variables[:core][:preprod_scheme]],
      ipa_name: ENV[env_variables[:core][:preprod_ipa_name]],
      export_method: "ad-hoc",
      google_service_info_plist_path: ENV[env_variables[:core][:preprod_info_plist]]
    )
  end

  lane :deploy_store_app_to_testflight do |options|
    deploy_to_testflight(
      target: "Store",
      app_identifier: ENV[env_variables[:core][:app_id]],
      scheme: ENV[env_variables[:core][:scheme]],
      ipa_name: ENV[env_variables[:core][:ipa_name]],
      export_method: "app-store",
      google_service_info_plist_path: ENV[env_variables[:core][:info_plist]]
    )
  end

  lane :sync_all_dev_certs do |options|
    sync_staging_dev_cert
    sync_preprod_dev_cert
    sync_prod_dev_cert
  end

  lane :sync_staging_dev_cert do |options|
    sync_dev_cert(
      target: "Staging",
      app_identifier: ENV[env_variables[:core][:staging_app_id]]
    )
  end

  lane :sync_preprod_dev_cert do |options|
    sync_dev_cert(
      target: "Preprod",
      app_identifier: ENV[env_variables[:core][:preprod_app_id]]
    )
  end

  lane :sync_prod_dev_cert do |options|
    sync_dev_cert(
      target: "Prod",
      app_identifier: ENV[env_variables[:core][:app_id]]
    )
  end

  error do |lane, exception, options|
    notify_slack_for_error(
      message: "🚑 Houston, we have a problem!",
      attachment_properties: {
        fields: [
          {
            title: "Git Tag",
            value: last_git_tag,
            short: true
          },
          {
            title: "Error",
            value: exception.to_s,
            short: false
          }
        ]
      }
    )
  end

  lane :deploy_to_testflight do |options|
    target = options[:target]
    app_identifier = options[:app_identifier]

    if app_identifier == nil
      UI.important "No app is found to deploy to Testflight [#{target}]"
      next
    end

    #1
    clean_build_artifacts

    #2
    register_connect_api_key

    #3
    sign(
      type: "appstore",
      app_identifier: app_identifier
    )

    #4
    archive(
        configuration: ENV[env_variables[:core][:app_store_build_config]],
        scheme: options[:scheme],
        output_name: options[:ipa_name],
        export_method: "app-store",
        is_store: true
    )

    #5
    upload_to_testflight(
      skip_submission: true, 
      skip_waiting_for_build_processing: true
    )

    #6
    upload_symbols_to_crashlytics(
      gsp_path: options[:google_service_info_plist_path],
      binary_path: ENV[env_variables[:core][:upload_symbols_path]]
    )

    #7
    notify_slack_for_success(
      message: "🚀 Released #{last_git_tag} to TestFlight!"
    )
  end

  lane :sync_dev_cert do |options|
    target = options[:target]
    app_identifier = options[:app_identifier]

    if app_identifier == nil
      UI.important "No app is found to sync development certificates [#{target}]"
      next
    end

    register_connect_api_key

    sign(
      type: "development",
      app_identifier: app_identifier
    )
  end

  # Sign Cerificates For Given Profile Type
  lane :sign do |options|
    match(
      type: options[:type], 
      app_identifier: options[:app_identifier],
      force_for_new_devices: true,
      verbose: true
    )
  end

  #Register api key for app store connect 
  lane :register_connect_api_key do |options|
    app_store_connect_api_key(
      key_id: ENV[env_variables[:match][:key_id]],
      issuer_id: ENV[env_variables[:match][:issuer_id]],
      key_content: ENV[env_variables[:match][:key_content]],
      in_house: ENV[env_variables[:match][:in_house]]
    )
  end

  lane :build do |options|
    #1
    scan(
      app_identifier: options[:app_identifier],
      scheme: options[:scheme],
      clean: true,
      build_for_testing: true
    )
  end

  lane :archive do |options|
    #1
    gym(
      configuration: options[:configuration],
      scheme: options[:scheme],
      clean: true,
      output_directory: "./archive",
      output_name: options[:output_name],
      include_bitcode: true,
      xcargs: {:BITCODE_GENERATION_MODE => "bitcode"},
      export_method: options[:export_method],
      verbose: true
    )
  end

  lane :notify_slack_for_success do |options|
    notify_slack(
      default_payloads: [],
      is_success: true,
      message: options[:message],
      attachment_properties: options[:attachment_properties]
    )
  end

  lane :notify_slack_for_error do |options|
    notify_slack(
      default_payloads: [:git_branch, :lane],
      is_success: false,
      message: options[:message],
      attachment_properties: options[:attachment_properties]
    )
  end

  lane :notify_slack do |options|
    slack_webhook_url = ENV[env_variables[:core][:slack_webhook_url]]

    if slack_webhook_url == nil
      UI.important "No 'Slack' webhook url is provided!"
      next
    end

    slack(
      slack_url: slack_webhook_url,
      default_payloads: options[:default_payloads],
      success: options[:is_success],
      message: options[:message],
      attachment_properties: options[:attachment_properties]
    )
  end
end

Issue with snapshot

https://docs.fastlane.tools/getting-started/ios/screenshots/



Problem: 

```
xcodebuild: error: Scheme AnniversaryTrackerSnapshot is not currently configured for the test action.
```

Fix:  https://stackoverflow.com/questions/30481630/xcode-project-scheme-is-not-currently-configured-for-the-test-action





https://github.com/CongL3/FastLaneToAppStore

| This has to be unique           |
| ------------------------------- |
| PRODUCE_SKU=TC_FASTLANETEST_001 |
|                                 |



#### Fastlane



```
fastlane init
4. 🛠  Manual setup - manually setup your project to automate your tasks

cd fastlane
touch .env
touch .env.ios
touch .env.secret

create gem file

source "https://rubygems.org"

gem "fastlane"
gem "dotenv"

bundle install

bundle exec fastlane create_app
```



properties([
            buildDiscarder(logRotator(numToKeepStr: '5')),
            parameters([
                    booleanParam(name: 'run_api_test', defaultValue: false),
                    booleanParam(name: 'run_ui_test', defaultValue: false),
                    booleanParam(name: 'run_mobile_test', defaultValue: false),
                    booleanParam(name: 'EnableANSIColor', defaultValue: true),
                    choice(name: 'ui_browser', choices: ['chrome', 'opera']),
                    string(name: "ui_browser_version", defaultValue: "127.0", trim: true),
                    string(name: "ui_selenoid_url", defaultValue: "http://192.168.1.124/wd/hub", trim: true),
                    string(name: "ui_image_version", defaultValue: "1.0.1", trim: true),
                    string(name: "api_image_version", defaultValue: "1.0.0", trim: true),
                    string(name: "mobile_platform_name", defaultValue: "android", trim: true),
                    string(name: "mobile_platform_version", defaultValue: "9.0", trim: true),
                    string(name: "mobile_automation_name", defaultValue: "UIAutomator2", trim: true),
                    string(name: "mobile_device_name", trim: true),
                    string(name: "mobile_udid", trim: true),
                    string(name: "mobile_appium_url", defaultValue: "http://192.168.56.1:4723", trim: true),
                    string(name: "mobile_apk_path", trim: true),
                    string(name: "mobile_image_version", defaultValue: "1.0.0", trim: true)

            ])
])

pipeline {

    agent {
        label 'docker-node-1'
    }

    options {
        timeout(time: 15, unit: 'MINUTES')
    }

    stages {
        stage("Build and Test") {
            parallel {
                stage("Run ui-tests") {
                    steps {
                        script {
                            if (params.run_ui_test) {
                                build job: 'ui-test',
                                wait: false,
                                parameters: [
                                    booleanParam(name: 'EnableANSIColor', value: "${EnableANSIColor}"),
                                    string(name: 'browser', value: "$ui_browser"),
                                    string(name: "browser_version", value: "$ui_browser_version"),
                                    string(name: "selenoid_url", value: "$ui_selenoid_url"),
                                    string(name: "image_version", value: "$ui_image_version")
                                ]
                            }
                        }
                    }
                }
                stage("Run api-tests") {
                    steps {
                        script {
                            if (params.run_api_test) {
                                build job: 'api-tests',
                                wait: false,
                                parameters: [
                                    booleanParam(name: 'EnableANSIColor', value: "${EnableANSIColor}"),
                                    string(name: 'image_version', value: "$api_image_version"),
                                ]
                            }
                        }
                    }
                }
                stage("Run mobile-test") {
                    steps {
                        script {
                            if (params.run_mobile_test) {
                                build job: 'mobile-tests',
                                wait: false,
                                parameters: [
                                    booleanParam(name: 'EnableANSIColor', defaultValue: "${EnableANSIColor}"),
                                    string(name: "platform_name", value: "$mobile_platform_name"),
                                    string(name: "platform_version", value: "$mobile_platform_version"),
                                    string(name: "automation_name", value: "$mobile_automation_name"),
                                    string(name: "device_name", value: "$mobile_device_name"),
                                    string(name: "udid", value: "$mobile_udid"),
                                    string(name: "appium_url", value: "$mobile_appium_url"),
                                    string(name: "apk_path", value: "$mobile_apk_path"),
                                    string(name: "image_version", value: "$mobile_image_version")
                                ]
                            }
                        }
                    }
                }
            }
        }
    }
}
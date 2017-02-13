#!/usr/bin/groovy

def environment='dev'

node {
    
stage 'CF-Build'
    
//    checkout([$class: 'GitSCM', branches: [[name: '*/master']], doGenerateSubmoduleConfigurations: false, extensions: [], submoduleCfg: [], userRemoteConfigs: [[credentialsId: 'ravienggtoo', url: 'https://github.com/ravienggtoo/cf-example.git']]])
    checkout scm
    sh "mvn clean resources:resources -P ${environment}"
    sh "cp -rf target/classes/manifest.yml ${WORKSPACE}/learn-bosh-release"
    
stage 'CFRelease'

    sh "cd ${WORKSPACE}/learn-bosh-release && bosh target 10.0.2.8"
    sh "cd ${WORKSPACE}/learn-bosh-release && bosh create release --force --name Jenkins-CF-Example --version ${BUILD_NUMBER}"
    sh "cd ${WORKSPACE}/learn-bosh-release && bosh upload release"
    sh "cd ${WORKSPACE}/learn-bosh-release && bosh releases"
    
stage 'CF-Deploy'
    sh "bosh deployment ${WORKSPACE}/learn-bosh-release/manifest.yml"
    sh "bosh -n deploy"
    sh "bosh vms"
}

// https://docs.qameta.io/allure/#_installing_a_commandline
// https://repo.maven.apache.org/maven2/io/qameta/allure/allure-commandline/2.8.0/
// configure allure tool at Jenkins level as well as on node, provide the path to the downloaded allure executable
// https://stackoverflow.com/questions/40379494/how-does-one-run-allure-plugin-in-jenkins-pipeline
// https://docs.qameta.io/allure/#_pytest

//http://allure.qatools.ru/ 

JOB_PARAMS = [
    srcLocation: "$HOME/pyworkspace/pytest-example",
    testsFolder: 'tests',
    testResultFolder: 'test-reports',
    testResultFile: 'pytest_unit.xml',
    allureTestResultFolder: 'allure-test-reports',
    allureTestResultFile: 'allure_pytest_unit.xml',
]

node('buildnode') {
    stage('xunit_test') {
        println('Pytest')
        perform_tests(JOB_PARAMS)
    }
    stage('allure_test') {
        println('Allure Pytest')
        perform_allure_tests(JOB_PARAMS)
    }
}


def perform_tests(JP) {

    dir ("${JP.srcLocation}/${JP.testResultFolder}") {
        deleteDir()
    }

    dir (JP.srcLocation) {
        retCode = sh label: 'perform tests', returnStatus: true, script: "python3 -m pytest --junit-xml=${JP.testResultFolder}/${JP.testResultFile} ${JP.testsFolder}"

        println('Test exec retCode: '+ retCode)
        xunit([JUnit(excludesPattern: '', pattern: "${JP.testResultFolder}/*.xml", stopProcessingIfError: true)])
    }
}


def perform_allure_tests(JP) {
    
    dir ("${JP.srcLocation}/${JP.allureTestResultFolder}") {
        deleteDir()
    }
    
    tstCmd = "python3 -m pytest --alluredir=${JP.allureTestResultFolder}"
    dir (JP.srcLocation) {
        retCode = sh label: 'perform tests', returnStatus: true, script: tstCmd

        allure([
                includeProperties: false,
                jdk: '',
                properties: [],
                reportBuildPolicy: 'ALWAYS',
                results: [[path: JP.allureTestResultFolder]]
        ])
    }

}

def projects = ["core/wb_sdram_ctrl", "system/sockit"]

node('docker-icarus-quartus') {
    try {
        stage 'Checkout'
        checkout scm
        
        stage 'Install'
        sh 'ls -la'
        sh "python setup.py install"
        sh "fusesoc init -y"
        
        stage 'Test'
        sh "fusesoc sim wb_sdram_ctrl"
        sh "fusesoc build sockit"

    } finally {
        stage 'Process reports'
        node {
            step([$class: 'LogParserPublisher', failBuildOnError: true, parsingRulesPath: '/var/lib/jenkins/userContent/logParser/default.txt', useProjectRule: false])
        }
    }
}


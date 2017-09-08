import org.librecores.ci.LCCI

node('docker-fusesoc-icarus') {
  try {
    stage("Build FuseSoC") {
      sh "whoami"
      new LCCI(this).checkoutScmOrFallback("https://github.com/oleg-nenashev/fusesoc.git")
      sh "pip install -e ."
      sh "fusesoc init -y"
    }
    
    // Run the existing Test suite
    stage("Test") {
      sh "fusesoc sim wb_sdram_ctrl"
    }
  } finally {
    stage("Process reports") {
      step([$class: 'LogParserPublisher', failBuildOnError: true, parsingRulesPath: "${env.JENKINS_HOME}/userContent/config/logParser/default.txt", useProjectRule: false])
    }
  }
}


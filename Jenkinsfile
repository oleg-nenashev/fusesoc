import org.librecores.ci.LCCI
import org.librecores.ci.Modules

def lcci_modules = new Modules(steps)

node('librecores-ci-modules') {
  try {
    stage("Init tools") {
      lcci_modules.load("eda/iverilog/v10_1")
    }
    
    stage("Build FuseSoC") {
      sh "whoami"
      new LCCI(this).checkoutScmOrFallback("https://github.com/oleg-nenashev/fusesoc.git")
      sh "pip install -e ."
      sh "fusesoc init -y"
    }
    
    // Run the existing Test suite
    stage("Test") {
      lcci_modules.sh "fusesoc sim wb_sdram_ctrl --iterations 10"
    }
  } finally {
    stage("Process reports") {
      step([$class: 'LogParserPublisher', failBuildOnError: true, parsingRulesPath: "${env.JENKINS_HOME}/userContent/config/logParser/default.txt", useProjectRule: false])
    }
  }
}


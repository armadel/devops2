// Just an example, for Rails config better use embedded ERB
final DATABASE_YAML_TEMPLATE = '''
test:
  adapter: oracle_enhanced
  database: app_db
  username: ${user}
  password: ${password}
'''
final CFG_PATH = 'config/database.yml'

def renderTemplate(template, bindings) {
  bindings.inject(template) { t, k, v -> t.replace("\${" + k + "}", v) }
}

pipeline {
  agent any
  environment {
      APP_DB = credentials('app_db')
  }
  stages {
    stage('Test') {
      steps {
        writeFile file: CFG_PATH,
                text: renderTemplate(DATABASE_YAML_TEMPLATE, ['user': env.APP_DB_USR, 'password': env.APP_DB_PSW])
        sh 'bundle exec rspec'
      }
    }
  }
  post {
    always {
      sh "rm -f ${CFG_PATH}"
    }
  }
}

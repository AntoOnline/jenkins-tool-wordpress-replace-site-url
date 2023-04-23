pipeline {
    agent any

    parameters {
        string(name: 'OLD_URL', defaultValue: 'https://oldurl.com', description: 'The old site URL')
        string(name: 'NEW_URL', defaultValue: 'https://newurl.com', description: 'The new site URL')
        string(name: 'MYSQL_USER', defaultValue: 'username', description: 'The MySQL user')
        string(name: 'MYSQL_PASSWORD', defaultValue: 'password', description: 'The MySQL password')
        string(name: 'MYSQL_DATABASE', defaultValue: 'database', description: 'The MySQL database name')
        string(name: 'MYSQL_HOST', defaultValue: 'mysql.server.com', description: 'The MySQL host')
        string(name: 'TABLE_PREFIX', defaultValue: 'wp_', description: 'The WordPress table prefix')
    }

    stages {

        stage('Check MySQL client') {
            steps {
                script {
                    def osName = sh(script: "uname -s", returnStdout: true).trim()
                    if (osName == 'Linux') {
                        sh 'dpkg -s mysql-client || (sudo apt-get update && sudo apt-get install mysql-client -y)'
                    } else {
                        error "Unsupported operating system: ${osName}"
                    }
                }
            }
        }

        stage('Replace URL') {
            steps {
                sh """
                    old_url='${params.OLD_URL}'
                    new_url='${params.NEW_URL}'
                    mysql_user='${params.MYSQL_USER}'
                    mysql_password='${params.MYSQL_PASSWORD}'
                    mysql_database='${params.MYSQL_DATABASE}'
                    mysql_host='${params.MYSQL_HOST}'
                    table_prefix='${params.TABLE_PREFIX}'

                    mysql -u \$mysql_user -p\$mysql_password -h \$mysql_host \$mysql_database -e "UPDATE \${table_prefix}options SET option_value = replace(option_value, '\$old_url', '\$new_url') WHERE option_name = 'home' OR option_name = 'siteurl';"
                    mysql -u \$mysql_user -p\$mysql_password -h \$mysql_host \$mysql_database -e "UPDATE \${table_prefix}posts SET guid = replace(guid, '\$old_url', '\$new_url');"
                    mysql -u \$mysql_user -p\$mysql_password -h \$mysql_host \$mysql_database -e "UPDATE \${table_prefix}posts SET post_content = replace(post_content, '\$old_url', '\$new_url');"
                    mysql -u \$mysql_user -p\$mysql_password -h \$mysql_host \$mysql_database -e "UPDATE \${table_prefix}postmeta SET meta_value = replace(meta_value, '\$old_url', '\$new_url');"
                """
            }
        }
    }
}

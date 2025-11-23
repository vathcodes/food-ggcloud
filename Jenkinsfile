pipeline {
    agent any

    triggers {
        pollSCM('H/5 * * * *')
    }

    environment {
        PORT = '4000'
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/vathcodes/food-ggcloud.git'
            }
        }

        stage('Build & Deploy Docker Compose') {
            steps {
                withCredentials([
                    string(credentialsId: 'MONGODB_URI', variable: 'MONGODB_URI'),
                    string(credentialsId: 'JWT_SECRET', variable: 'JWT_SECRET'),
                    string(credentialsId: 'STRIPE_SECRET_KEY', variable: 'STRIPE_SECRET_KEY')
                ]) {
                    sh '''
                    # Tạo file .env cho backend
                    cat > .env <<EOF
PORT=$PORT
MONGODB_URI=$MONGODB_URI
JWT_SECRET=$JWT_SECRET
STRIPE_SECRET_KEY=$STRIPE_SECRET_KEY
EOF

                    # Build & Run
                    docker-compose down
                    docker-compose up --build -d
                    '''
                }
            }
        }
    }
}

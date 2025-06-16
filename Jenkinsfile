pipeline{
    agent any

    environment{
        CHROME_VERSION =  '137.0.7151.104';
        CHROMEDRIVER_VERSION = "137.0.7151.7000";
        CHROME_INSTALL_PATH = 'C:\\Program Files\\Google\\Chrome\\Application'
        CHROMEDRIVER_PATH = 'C:\\Program Files\\Google\\Chrome\\Application\\chromedriver-win64\\chromedriver.exe'
    }

    stages{
        stage("Checkout Code"){
            steps{
                //Checkout code from GitHub and specify the branch
                git branch: 'main', url: 'https://github.com/b-angelov/SeleniumIDE-Jenkins.git';
            }
        }

        stage("Setup .NET Core"){
            steps{
                  bat '''
                     echo Installing .NET SDK 6.0
                     choco install dotnet-sdk -y --version=6.0.100
                  '''
            }
        }

        stage("Restore dependencies"){
            steps{
                //Restore dependencies using the solution File
                bat 'dotnet restore SeleniumIde.sln'
            }
        }

        stage("Build"){
            steps{
                //Build the project
                bat 'dotnet build SeleniumIde.sln --configuration Release'
            }
        }

        stage("Run Test"){
            steps{
                //Run test using the solution file with loggin
                bat 'dotnet test SeleniumIde.sln --logger "trx;LogFileName=TestResults.trx"'
                }
         }
    }

     post{
        always{
            archiveArtifacts artifacts: '**/TestResults/*.trx', allowEmptyArchive: true;
            junit '**/TestResults/*.trx'
        }
     }
}
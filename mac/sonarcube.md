# install sonarcube on mac

```sh

brew install sonar
brew install sonar-scanner

```

## edit on .zshrc 
```sh
export SONAR_HOME=/usr/local/Cellar/sonar-scanner/{version}/libexec 
export SONAR=$SONAR_HOME/bin export PATH=$SONAR:$PATH

```

## start 
```
brew services start sonarqube

```

## login on website
```
Enter http://localhost:9000 in the browser to enter the following page. 

Log in to SonarQube and enter the account and password admin/admin


```
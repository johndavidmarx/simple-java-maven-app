name: Java CI/CD Pipeline Example

on: [push]
jobs:
  build:

    runs-on: ubuntu-latest

    steps:

    - uses: actions/checkout@v1

#    - name: Set up JDK 1.8
#      uses: actions/setup-java@v1
#      with:
#        java-version: 1.8

    - name: Prepare pem file
      run: gpg --quiet --batch --yes --decrypt --passphrase="$GPGPW" --output marx4universe.pem secrets/marx4universe.pem.gpg
      env:
        GPGPW: ${{secrets.awspassword}}        
    - name: Change pem File Permissions
      run: chmod 0400 marx4universe.pem
      
    - name: aws
      run: aws --version
    - name: aws 
      run: aws s3 ls --region=us-east-2
      env:
        AWS_ACCESS_KEY_ID: ${{ secrets.awskeyid }}
        AWS_SECRET_ACCESS_KEY: ${{ secrets.awssecret }}

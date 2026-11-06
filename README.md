# name: CI Build

on:
  push:
    branches: [ "main" ]
  pull_request:
    branches: [ "main" ]

jobs:
  build:
    runs-on: ubuntu-latest

    steps:
    - name: Checkout Code
      uses: actions/checkout@v3

    - name: Set up Python
      uses: actions/setup-python@v4
      with:
        python-version: '3.9'
        cache: 'pip'

    - name: Install Dependencies
      run: |
        python -m pip install --upgrade pip
        pip install -r requirements.txt

    - name: Lint with Flake8 and Pylint
      run: |
        flake8 service/ tests/ --count --select=E9,F63,F7,F82 --show-source --statistics
        pylint service/ tests/

    - name: Run Unit Tests with Nosetests
      run: |
        nosetests --with-spec --spec-color --with-coverage --cover-package=service

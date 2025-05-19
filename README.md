# GreenPythonWorkflow

```

name: "CodeQL"

on:
  push:
    branches: [master]
  pull_request:
    branches: [master]

permissions:
  contents: read
  security-events: write



jobs:
  analyze:
    name: Analyze
    runs-on: ubuntu-latest
    strategy:
      matrix:
        language: [python]

    steps:

    - name: Start eco ci energy measurement
      uses: green-coding-solutions/eco-ci-energy-estimation@v4
      with:
        task: start-measurement

    
    - name: Checkout repository
      uses: actions/checkout@v3

    - name: Setup Initialise CodeQL measurement
      uses: green-coding-solutions/eco-ci-energy-estimation@v4
      with:
        task: get-measurement
        label: 'initialize codeql'  

    - name: Initialize CodeQL
      uses: github/codeql-action/init@v3
      with:
        languages: python
        packs: timh1975/greenpython@2.0.3
    
    - name: Performance CodeQL Analysis measurement
      uses: green-coding-solutions/eco-ci-energy-estimation@v4
      with:
          task: get-measurement
          label: 'Perform CodeQL Analysis'  

    - name: Perform CodeQL Analysis
      uses: github/codeql-action/analyze@v3
      with:
          output: results.sarif 
          category: python/example

    - name: Setup Upload Sarif as artifact measurement
      uses: green-coding-solutions/eco-ci-energy-estimation@v4
      with:
        task: get-measurement
        label: 'Upload SARIF measurement'       
         
    - name: Upload SARIF file as artifact (for download)
      uses: actions/upload-artifact@v4
      with:
        name: codeql-sarif-results
        path: results.sarif  

    - name: Install sarif-filter measurement
      uses: green-coding-solutions/eco-ci-energy-estimation@v4
      with:
        task: get-measurement
        label: 'install sarif-filter measurement'      
 
    - name: Install sarif-filter
      run: pip install -i https://test.pypi.org/simple/ sarif-filter

    - name: Filter Sarif file Measurement
      uses: green-coding-solutions/eco-ci-energy-estimation@v4
      with:
        task: get-measurement
        label: 'filter sarif file measurement'

    - name: Filter SARIF file
      run: sarif_filter results.sarif/python.sarif green-python

    - name: Setup Upload Sarif as artifact measurement
      uses: green-coding-solutions/eco-ci-energy-estimation@v4
      with:
        task: get-measurement
        label: 'Upload SARIF measurement'      

    - name: Upload filtered SARIF file as artifact (for download)
      uses: actions/upload-artifact@v4
      with:
        name: filtered-sarif-file
        path: results.sarif

    - name: Show Energy Results
      uses: green-coding-solutions/eco-ci-energy-estimation@v4
      with:
        task: display-results

    - name: Print total data
      run: |
          echo "total json: ${{ steps.total-measurement-step.outputs.data-total-json }}"    
    
  ```

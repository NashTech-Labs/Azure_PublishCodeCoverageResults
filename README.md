# Azure_PublishCodeCoverageResults
Use this task to publish Cobertura or JaCoCo code coverage results from a build.  This task is used to get code coverage results from a build.

## Pipeline Requirements

The dotNet Module pipeline requires the following parameters to be defined:

Parameters:

| Name  | Displayname | type | Default | Values | Opional/Required | Comments |
| ------------- | ------------- | :-------------: | :-------------: | :-------------: | :-------------: | ------------- |
| codeCoverageTool | Code coverage tool | String | 'JaCoCo' | 'JaCoCo' / 'Cobertura' | Required | Specifies the tool that generates code coverage results |
| summaryFileLocation | Path of the summary file | String | "**/coverage.cobertura.xml" |  | Required | Specifies the path of the summary file containing code coverage statistics, such as line, method, and class coverage |
| pathToSources | Path to Source files | String | '' |  | Optional | Specifying a path to source files is required when coverage XML reports don't contain an absolute path to source files |
| reportDirectory | Report directory | String | '' |  | Optional | Specifies the path of the code coverage HTML report directory. The report directory is published for later viewing as an artifact of the build |
| additionalCodeCoverageFiles | Additional files | String | '' |  | Optional | Specifies the file path pattern and notes any additional code coverage files to be published as artifacts of the build |
| failIfCoverageEmpty | Fail when code coverage results are missing | Boolean | false | true / false | Optional | Fails the task if code coverage did not produce any results to publish |
|

These parameters provide multiple use case options for the dotnet templates pipeline, enable/disable flags for the utilization of different templates as per the requirements.


## Use Cases

You can directly call a particular template as per the requirement. for example: 

  ```yaml
  # azure-pipeline.yml
  resources:
  repositories:
    - repository: Template
      type: github
      name: your_username/Azure_PublishCodeCoverageResults
      ref: <respective branch name>
      endpoint: 'githubServiceConnectioNname'

  steps:
  # passing the parameters
  - ${{ if eq( parameters.codeCoverageTool, 'JaCoCo') }}:
    - template: PublishCodeCoverageResults.yaml
      parameters:
        codeCoverageTool: '${{ parameters.codeCoverageTool }}'
        summaryFileLocation: '${{ parameters.summaryFileLocation}}' #"$(Agent.TempDirectory)/**/coverage.${{ parameters.codeCoverageTool }}.xml"
        pathToSources: '${{ parameters.pathToSources }}'
        reportDirectory: '${{ parameters.reportDirectory }}' 
        additionalCodeCoverageFiles: '${{ parameters.additionalCodeCoverageFiles }}' 
        failIfCoverageEmpty: '${{ parameters.failIfCoverageEmpty }}'
  - ${{ elseif eq( parameters.codeCoverageTool, 'Cobertura') }}:
    - template: PublishCodeCoverageResults.yaml
      parameters:
        codeCoverageTool: '${{ parameters.codeCoverageTool }}'
        summaryFileLocation: '${{ parameters.summaryFileLocation }}'
        pathToSources: '${{ parameters.pathToSources }}'
        reportDirectory: '${{ parameters.reportDirectory }}' 
        additionalCodeCoverageFiles: '${{ parameters.additionalCodeCoverageFiles }}' 
        failIfCoverageEmpty: '${{ parameters.failIfCoverageEmpty }}'                

  ```
Make sure to adjust the repository name, branch name, and parameter values according to your project's requirements.

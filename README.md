# s3-multipart-upload-maven-plugin
Maven plugin to allow multipart uploads of large files via the low level Java SDK.

## Configuration parameters

| Parameter | Description | Required | Default |
|-----------|-------------|----------|---------|
|bucket|The name of the bucket. |*yes*| |
|source|The source file. |*yes*| |
|key|The destination key in s3. | *yes*| |
|profile|AWS Profile | *no* | if unspecified, the plugin falls back to environment variables. |
|chunkSize|The number of chunks into which to split the source | *no* | 20 |

## Example plugin definition
```xml
  <build>
    <plugins>
      <plugin>
        <groupId>io.thinkstack.maven.plugins</groupId>
        <artifactId>s3-multipart-upload-maven-plugin</artifactId>
        <version>1.0.2</version>
        <configuration>
          <bucket>s3-bucket</bucket>
          <source>/tmp/test.zip</source>
          <key>test.zip</key>
          <profile>your-aws-profile</profile>
          <chunkSize>20</chunkSize>
        </configuration>
      </plugin>
    </plugins>
  </build>
```
## Example Maven command
```bash
mvn io.thinkstack.maven.plugins:s3-upload-maven-plugin:1.0.2:s3-multipart-upload
```

## Running Unit Tests

### AWS Profile
The test resources use the profile `io.thinkstack` so either change this to a value already configured or add a new AWS 
profile.

### IntelliJ Setup
When running the unit tests in IntelliJ I have had to add the following commands _before_ the tests run.

* Run | Edit Configurations...
* Select JUnit Profile
* Add the following to the `Before launch:` panel

#### Run Maven Goal
This command ensures the javadoc descriptors are present before the mojo tests are run
```bash
mvn clean install -DskipTests -DskipITs
```

#### Run External Tool
This setup ensures that the 120mb size file is present. It can't be added to the project as it breaks GitHub's 
file size limit of 100mb
* Program = `/bin/dd`
* Parameters = `dd if=/dev/zero of=src/test/resources/120mb.dat  bs=125829120  count=1`
* Working directory = `~/path/to/source/DeploymentKing/s3-multipart-upload-maven-plugin`

## Deployment
This project has not been deployed to Maven Central. The project has no distribution management section so it is the 
responsibility of anyone using this repo, to deploy it appropriately to their own repo using the following:

```bash
mvn deploy -DaltDeploymentRepository=repo-id::default::https://repo-url
```

## References
* [docs.aws.amazon.com: Upload a File (S3)](http://docs.aws.amazon.com/AmazonS3/latest/dev/llJavaUploadFile.html)
* [stackoverflow.com: Upload static resource to Amazon s3 server using maven plugin](https://stackoverflow.com/questions/20650514/upload-static-resource-to-amazon-s3-server-using-maven-plugin)
* [dzone.com: Amazon S3 Parallel MultiPart File Upload](https://dzone.com/articles/amazon-s3-parallel-multipart)

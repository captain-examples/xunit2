# Getting Captain working with xUnit

Starting from a [simple workflow that runs xunit][workflow-before-captain], we want to

1. 🧪 Ensure xUnit produces xUnit-xml output

Ensure your project includes the [`XunitXml.TestLogger`](https://www.nuget.org/packages/XunitXml.TestLogger) test logger

```sh
dotnet add package XunitXml.TestLogger --version 3.0.70
```

And use it with `xunit test --logger xunit`. Note: you can also change the outputted filename.

```sh
dotnet test --logger "xunit;LogFileName=TestResults.xml" --results-directory "TestResults"
```

Note: I've specified the filename and results directory here, however `TestResults/TestResults.xml` is also the default
result path

2. 🔐 Create an API token

Create an API token for your organization within [captain][captain], ([more documentation here][create-api-token]).
The token needs write access.

Add the new token as an action secret to your repository. Conventionally, we call this secret `CAPTAIN_API_TOKEN`.

3. 💌 Add a step to upload to captain

```yaml
- name: Upload test results to Captain # optional, shows in github action results
  uses: rwx-research/upload-captain-artifact@v1
  if: always() # run even if build fails
  continue-on-error: true # if upload to captain fails, don't fail the build
  with:
    artifacts: |
      [
        {
          "name": "xUnit",
          "path": "TestResults/TestResults.xml",
          "kind": "test_results",
          "parser": "xunit_dot_net_xml"
        }
      ]
    captain-token: '${{ secrets.CAPTAIN_API_TOKEN }}'
```

4. 🎉 See your test results in Captain!

Take a look at the [final workflow!][workflow-with-captain]

For more information on the github action, see [its readme][action-readme].

[workflow-before-captain]: https://github.com/captain-examples/xunit2/blob/basic-workflow/.github/workflows/ci.yml
[captain]: https://captain.build/_/organizations
[create-api-token]: https://www.rwx.com/captain/docs/api-tokens#generating-an-api-token
[workflow-with-captain]: https://github.com/captain-examples/xunit2/blob/main/.github/workflows/ci.yml
[action-readme]: https://github.com/rwx-research/upload-captain-artifact/#readme

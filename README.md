# Getting Captain working with xUnit

Starting from a [simple workflow that runs xunit][workflow-before-captain], we want to

## 1. 🧪 Ensure xUnit produces xUnit-xml output

Ensure your project includes the [`XunitXml.TestLogger`](https://www.nuget.org/packages/XunitXml.TestLogger) test logger

```sh
dotnet add package XunitXml.TestLogger --version 3.0.70
```

And use it with `xunit test --logger xunit`. Note: you can also change the outputted filename.

```sh
dotnet test --logger "xunit;LogFileName=TestResults.xml" --results-directory "TestResults"
```

Note: We've specified the filename and results directory here, however `TestResults/TestResults.xml` is also the default
result path

## 2. 🔐 Create an Access Token

Create an Access Token for your organization within [Captain][captain] ([more documentation here][create-access-token]).

Add the new token as an action secret to your repository. Conventionally, we call this secret `RWX_ACCESS_TOKEN`.

## 3. 💌 Install the Captain CLI, and call it when running tests

See the [full documentation on test suite integration][test-suite-integration].

```yaml
- uses: rwx-research/setup-captain@v1
- name: Run tests
  run: |
    captain run \
      --suite-id captian-examples-xunit2 \
      --test-results TestResults/TestResults.xml \
      -- dotnet test --logger "xunit;LogFileName=TestResults.xml" --results-directory "TestResults"
  env:
    RWX_ACCESS_TOKEN: ${{ secrets.RWX_ACCESS_TOKEN }}
```

## 4. 🎉 See your test results in Captain!

Take a look at the [final workflow!][workflow-with-captain]

[workflow-before-captain]: https://github.com/captain-examples/xunit2/blob/basic-workflow/.github/workflows/ci.yml
[captain]: https://account.rwx.com/deep_link/manage/access_tokens
[create-access-token]: https://www.rwx.com/docs/access-tokens
[workflow-with-captain]: https://github.com/captain-examples/xunit2/blob/main/.github/workflows/ci.yml
[test-suite-integration]: https://www.rwx.com/captain/docs/test-suite-integration

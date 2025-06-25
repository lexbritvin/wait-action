# ⏳ Wait for Job, Artifact, File Action

> 🚀 Wait for jobs (matrix support + regex patterns), artifacts, or files with intelligent matching and configurable timeouts.

## ✨ Features

- 📁 **File Watching** - Wait for files to appear on disk
- 📦 **Artifact Monitoring** - Track GitHub artifacts completion
- 🔄 **Job Synchronization** - Coordinate multiple workflow jobs with matrix support
- 🎯 **Smart Job Matching** - Flexible job name matching (exact, prefix, regex patterns)
- 🏗️ **Matrix Job Support** - Handle complex job matrices seamlessly
- 🪄 **Regex Patterns** - Advanced pattern matching for dynamic job names
- ⏰ **Smart Timeouts** - Configurable timeouts with polling
- 📊 **Rich Feedback** - Detailed status messages with job completion details

## 🚀 Quick Start


### 🔄 Wait for Jobs

```yaml
# 🧪 Wait for all test jobs
- name: ⚡ Wait for tests to complete
  uses: lexbritvin/wait-action@v1
  with:
    condition-type: 'job'
    job-name: 'test'
    timeout-seconds: 900

# 🏗️ Wait with regex pattern
- name: 🎨 Wait for build matrix
  uses: lexbritvin/wait-action@v1
  with:
    condition-type: 'job'
    job-name: '/build-\d+/'
    timeout-seconds: 1200
```

### 📦 Wait for Artifact

```yaml
- name: 🎯 Wait for test results
  uses: lexbritvin/wait-action@v1
  with:
    condition-type: 'artifact'
    artifact-name: 'test-coverage'
    timeout-seconds: 600
```

### 📁 Wait for File

```yaml
- name: 📄 Wait for build output
  uses: lexbritvin/wait-action@v1
  with:
    condition-type: 'file'
    file-path: './dist/bundle.js'
    timeout-seconds: 300
```

## ⚙️ Configuration

| Input                   | Description                               | Required | Default | Example             |
|-------------------------|-------------------------------------------|:--------:|---------|---------------------|
| `condition-type`        | 🎯 **Type**: `file`, `artifact`, or `job` |   Yes    | -       | `job`               |
| `file-path`             | 📁 File path to monitor                   |    No    | -       | `./dist/app.js`     |
| `artifact-name`         | 📦 Artifact to track                      |    No    | -       | `build-assets`      |
| `job-name`              | 🔄 Job name or pattern                    |    No    | -       | `test` or `/ci-.*/` |
| `timeout-seconds`       | ⏰ Max wait time in seconds                |    No    | `1800`  | `900`               |
| `poll-interval-seconds` | 🔄 Check frequency                        |    No    | `10`    | `5`                 |

## 🎯 Job Pattern Magic

| Pattern Type  | Example        | Matches                                 |
|---------------|----------------|-----------------------------------------|
| 🎯 **Exact**  | `deploy-prod`  | Only "deploy-prod"                      |
| 🔍 **Prefix** | `test`         | "test", "test (node-16)", "test-ubuntu" |
| 🪄 **Regex**  | `/test.*node/` | "test-with-node", "testing-node-16"     |

### 🌟 Pattern Examples

```yaml
job-name: 'test'           # 🔍 All test jobs
job-name: '/build-\d+/'    # 🔢 build-1, build-42
job-name: '/^(ci|test)-/'  # 🎭 Jobs starting with ci- or test-
```

## 📤 Outputs

- 🎯 **`result`**: `success` | `timeout` | `error`
- 💬 **`message`**: Detailed status with job information

### 📝 Example Messages

```
✅ All 3 job(s) completed: test (node-16), test (node-18), test (node-20)
⏳ 2/3 job(s) still running: build (windows), build (macos)
❌ No jobs found matching 'deploy'. Available: test, build, lint
```

## 🏗️ Real-World Example

```yaml
name: 🚀 Smart Deploy Pipeline
on: [ push ]

jobs:
  test:
    name: Test Suite
    runs-on: ubuntu-latest
    strategy:
      matrix:
        node: [ 16, 18, 20 ]
        os: [ ubuntu-latest, windows-latest ]
    steps:
      - run: npm test

  build:
    name: Build Assets
    runs-on: ubuntu-latest
    steps:
      - run: npm run build

  deploy:
    name: 🚀 Deploy to Production
    runs-on: ubuntu-latest
    steps:
      - name: ⏳ Wait for all tests & builds
        uses: lexbritvin/wait-action@v1
        with:
          condition-type: 'job'
          job-name: '/(Test Suite|Build Assets)/'
          timeout-seconds: 900

      - name: 🚀 Deploy application
        run: echo "🎉 Deploying to production!"
```

## 🎨 Why Choose This Action?

- ✨ **Zero Configuration** - Works out of the box
- 🔧 **Highly Flexible** - Multiple condition types
- ⚡ **Performance Focused** - Efficient polling
- 🛡️ **Error Resilient** - Graceful timeout handling
- 📊 **Rich Feedback** - Detailed status messages

## 📄 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

---

**⭐ Star this repo if you find it useful!**

Made with ❤️ for the GitHub Actions community

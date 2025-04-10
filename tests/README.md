# Posterizarr Tests

This directory contains tests for the Posterizarr project, with a focus on testing the Start.ps1 script and the trigger.py script used for Tautulli integration.

## Test Structure

The tests are organized into the following directories:

- `docker/`: tests to validate the docker container
- `unit/`: Unit tests for individual functions in Start.ps1
- `functional/`: Functional tests for different execution modes
- `integration/`: Integration tests that verify interactions with other components
- `mocks/`: Mock files used by the tests

## Running Tests

To run the tests, you need to have PowerShell, Python, and Pester installed. Pester is a testing framework for PowerShell.

### Installing Pester

If you don't have Pester installed, you can install it using the following command:

```powershell
Install-Module -Name Pester -Force -SkipPublisherCheck -Scope CurrentUser
```

### Using VSCode Test Explorer

The project uses VSCode's test explorer to run Pester and Python tests. To use it:

1. Install the recommended extensions:
   - PowerShell Extension (`ms-vscode.powershell`)
   - Pester Test Explorer (`pspester.pester-test`)
   - Python Extension (`ms-python.python`)
   - Pylance (`ms-python.vscode-pylance`)

2. Open the Test Explorer view in VSCode (click on the flask icon in the sidebar)

3. You should see all Pester tests and Python unittest tests listed in the explorer

4. Click on the play button next to a test to run it, or use the play buttons at the top to run all tests

5. To run tests with code coverage, use the "Run All Tests with Coverage" button in the Test Explorer toolbar

The project includes VSCode workspace settings (in `.vscode/settings.json`) that configure the Pester Test Explorer extension to use the `.pester.yml` configuration file and enable code coverage reporting.

### Code Coverage Reports

The project is configured to generate code coverage reports when running Pester tests. This helps identify which parts of the code are being tested and which parts need more test coverage.

To generate a code coverage report:

1. Run the tests using the "Run All Tests with Coverage" button in the VSCode Test Explorer toolbar

2. The coverage report will be generated in the `coverage.xml` file in JaCoCo format

3. You can view this report using various tools:
   - Install the "Coverage Gutters" VSCode extension to see coverage directly in your editor
   - Use a JaCoCo report viewer to see detailed coverage metrics
   - Convert to HTML format for a more readable report using tools like ReportGenerator

The coverage configuration is defined in the `.pester.yml` file in the root directory and includes:
- Coverage for Start.ps1 (Posterizarr.ps1 coverage is commented out as it's not ready yet)
- Exclusion of test files from coverage metrics
- Output in JaCoCo XML format for compatibility with various tools

#### Troubleshooting Code Coverage

If the "Run All Tests with Coverage" button doesn't work:

1. Make sure you have the latest version of the Pester Test Explorer extension installed
2. Try restarting VSCode to reload the extensions
3. Check that the `.pester.yml` file is correctly formatted
4. Verify that the `.vscode/settings.json` file contains the correct configuration for code coverage
5. Run tests manually with coverage using the PowerShell terminal:

   ```powershell
   Invoke-Pester -Configuration (Get-Content -Path .pester.yml | ConvertFrom-Yaml)
   ```

6. Look for any error messages in the VSCode output panel (View > Output > Pester Test Explorer)

### Running Python Tests from Command Line

To run the Python unit tests, navigate to the tests directory and run:

```bash
python unit/Trigger.Tests.py
```

## Test Files

### PowerShell Tests
- `unit/Start.Tests.ps1`: Unit tests for individual functions in Start.ps1
- `functional/Modes.Tests.ps1`: Tests for different execution modes (run, watch, scheduled)
- `integration/Integration.Tests.ps1`: Tests for interactions with Posterizarr.ps1 and the environment
- `integration/Trigger_Integration.Tests.ps1`: Tests for the integration between trigger.py and Start.ps1

### Python Tests
- `unit/Trigger.Tests.py`: Unit tests for the trigger.py script used for Tautulli integration

### Mock Files
- `mocks/MockPosterizarr.ps1`: A mock version of Posterizarr.ps1 for testing
- `mocks/test-config.json`: A test configuration file

### Docker Tests
- `docker/test.sh`: Shell script for testing the Docker container
- `docker/tests.yaml`: YAML configuration for Docker tests

## Adding New Tests

When adding new tests, follow these guidelines:

1. Place unit tests in the `unit/` directory
2. Place functional tests in the `functional/` directory
3. Place integration tests in the `integration/` directory
4. Use descriptive test names that clearly indicate what is being tested
5. Use the Pester `Describe`, `Context`, and `It` blocks to organize tests
6. Use mocks to isolate the code being tested
7. Use the `TestDrive` PSDrive for temporary files and directories
8. For testing the watch mode, use the `-Timeout` parameter to avoid infinite loops:
   ```powershell
   # Example: Run watch mode for 5 seconds in tests
   & "$PSScriptRoot/../../Start.ps1" -Mode watch -Timeout 5
   ```

## Best Practices

- Keep tests independent of each other
- Clean up any resources created during tests
- Use mocks to avoid external dependencies
- Write tests that are easy to understand and maintain
- Focus on testing behavior, not implementation details
- Use descriptive error messages

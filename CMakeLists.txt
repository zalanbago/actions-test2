# Set the CMP0048 policy to NEW to allow VERSION in project() command
cmake_policy(SET CMP0048 NEW)

# Define the project name and version
project(MyProject VERSION 1.0.0)

# Set a dummy PRODUCT_VERSION
set(PRODUCT_VERSION "1.0.0")

# Check if running on GitHub Actions
if(DEFINED ENV{GITHUB_ACTIONS} AND $ENV{GITHUB_ACTIONS} STREQUAL "true")
    if(WIN32)
        # On Windows, use PowerShell to append to GITHUB_ENV
        execute_process(
            COMMAND PowerShell -Command "[System.IO.File]::AppendAllText([System.Environment]::GetEnvironmentVariable('GITHUB_ENV'), 'elso=${PRODUCT_VERSION}`n')"
        )
        execute_process(
            COMMAND PowerShell -Command "
                $githubEnvPath = [System.Environment]::GetEnvironmentVariable('GITHUB_ENV')
                Add-Content -Path $githubEnvPath -Value 'masodik=${PRODUCT_VERSION}'
            "
        )
        execute_process(
            COMMAND PowerShell -Command "[System.IO.File]::AppendAllText([System.Environment]::GetEnvironmentVariable('GITHUB_ENV'), 'harmadik=1.1.1`n')"
        )
    else()
        # On Unix-based systems (macOS, Linux), use bash to append to GITHUB_ENV
        execute_process(
            COMMAND bash -c "echo 'buildNumber=${PRODUCT_VERSION}' >> $GITHUB_ENV"
        )
    endif()
endif()

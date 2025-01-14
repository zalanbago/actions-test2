project(MyProject VERSION 1.0.0)

cmake_minimum_required(VERSION 3.10)

# Set a dummy PRODUCT_VERSION
set(PRODUCT_VERSION "1.0.0")

# Check if running on GitHub Actions
if(DEFINED ENV{GITHUB_ACTIONS} AND $ENV{GITHUB_ACTIONS} STREQUAL "true")
    if(WIN32)
        # On Windows, use PowerShell to append to GITHUB_ENV
        execute_process(
            COMMAND PowerShell -Command "[System.IO.File]::AppendAllText('$env:GITHUB_ENV', 'buildNumber=${PRODUCT_VERSION}`n')"
        )
    else()
        # On Unix-based systems (macOS, Linux), use bash to append to GITHUB_ENV
        execute_process(
            COMMAND bash -c "echo 'buildNumber=${PRODUCT_VERSION}' >> $GITHUB_ENV"
        )
    endif()
endif()

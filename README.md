# Setup

## Hugo

1. Clone the repository
1. Install Hugo (Check for newest version)
    ```bash
    sudo snap install hugo
    ```

## Theme

1. Initialize submodule
    ```bash
    git submodule init
    ```
1. Update submodule
    ```bash
    git submodule update --remote --merge
    ```

# Hugo Commands

- Start server
    ```bash
    hugo server -D
    ```
- Create new recipe
    ```bash
    hugo new content recipes/<recipe>/index.md
    ```
- Build website
    ```bash
    hugo
    ```




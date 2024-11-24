# Use the official Ubuntu base image
FROM ubuntu:20.04

# Set environment variables to prevent interactive prompts
ENV DEBIAN_FRONTEND=noninteractive

# Install required dependencies
RUN apt-get update && apt-get install -y \
    curl \
    sudo \
    ca-certificates \
    tar \
    lsb-release \
    && rm -rf /var/lib/apt/lists/*

# Install Node.js (required for GitHub Actions Runner)
RUN curl -fsSL https://deb.nodesource.com/setup_16.x | sudo -E bash - && \
    sudo apt-get install -y nodejs

# Create a non-root user to run the GitHub Actions runner
RUN useradd -m runner

# Switch to the non-root user
USER runner
WORKDIR /home/runner

# Download the GitHub Actions runner
RUN curl -o actions-runner-linux-x64-2.320.0.tar.gz -L https://github.com/actions/runner/releases/download/v2.320.0/actions-runner-linux-x64-2.320.0.tar.gz

# Extract the runner package
RUN tar xzf actions-runner-linux-x64-2.320.0.tar.gz

# Expose port for GitHub runner communication (optional, adjust if necessary)
EXPOSE 8080

# Set entrypoint to configure the runner and start it
CMD ./config.sh --url https://github.com/YourUsername/YourRepo --token $GITHUB_TOKEN && ./run.sh

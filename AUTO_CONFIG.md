# Automatic Configuration Setup

This guide explains how to set up automatic GitHub repository configuration for the SillyTavern Cloud Saves plugin, eliminating the need for manual UI configuration.

## Prerequisites

1. A GitHub personal access token with repository permissions
2. An existing GitHub repository for your cloud saves
3. The SillyTavern Cloud Saves plugin installed

## Setup Instructions

### Option 1: Using GIT_CONFIG_PATH (Recommended)

Create a git configuration file at the path specified by `GIT_CONFIG_PATH`. The plugin will automatically discover this location and display it during initialization.

**Default location**: `<SillyTavern_root>/.git/git_autosave_config.json`

Create the file with this minimal structure:

```json
{
  "repo_url": "https://github.com/your-username/your-repo.git",
  "github_token": "ghp_your_github_personal_access_token_here",
  "branch": "main"
}
```

This approach keeps your git configuration separate from the main plugin config and provides cleaner separation of concerns.

### Option 2: Using config.json (Legacy)

In the plugin directory, create or edit the `config.json` file with the following structure:

```json
{
  "repo_url": "https://github.com/your-username/your-repo.git",
  "github_token": "ghp_your_github_personal_access_token_here",
  "branch": "main",
  "display_name": "Your Display Name",
  "is_authorized": false,
  "username": "",
  "last_save": null,
  "current_save": null,
  "has_temp_stash": false,
  "autoSaveEnabled": false,
  "autoSaveInterval": 30,
  "autoSaveTargetTag": ""
}
```

## Required Configuration Fields

For automatic configuration to work with either method, you **must** set these three fields:

- `repo_url`: Your GitHub repository URL (HTTPS format)
- `github_token`: Your GitHub personal access token
- `branch`: The branch to use (typically "main" or "master")

## Optional Configuration Fields (config.json only)

When using Option 2 (config.json), you can also configure:

- `display_name`: A display name for the plugin UI
- `autoSaveEnabled`: Enable automatic saves (true/false)
- `autoSaveInterval`: Interval in minutes for automatic saves
- `autoSaveTargetTag`: Tag name for automatic save overwrites

## GitHub Token Setup

1. Go to GitHub Settings > Developer settings > Personal access tokens
2. Generate a new token with these permissions:
   - `repo` (Full control of private repositories)
   - `contents:write` (if using fine-grained tokens)
3. Copy the token and paste it into the `github_token` field

## Start SillyTavern

When you restart SillyTavern, the plugin will automatically:

1. Check for git configuration in GIT_CONFIG_PATH first
2. Fallback to config.json if no git config found
3. Initialize the Git repository if needed
4. Configure the remote repository
5. Test the connection
6. Mark the repository as authorized

You'll see console output like:
```
[cloud-saves] 💡 提示：如需自动配置，请在 GIT_CONFIG_PATH 或 config.json 中设置 repo_url、github_token 和 branch
[cloud-saves] 💡 GIT_CONFIG_PATH: /path/to/sillytavern/.git/git_autosave_config.json
[cloud-saves] 检查是否需要自动配置远程仓库...
[cloud-saves] Valid git config found: repo=https://github.com/user/repo.git, branch=main
[cloud-saves] 使用 GIT_CONFIG_PATH 中的配置进行自动配置...
[cloud-saves] 开始自动配置过程...
[cloud-saves] ✅ 自动配置完成！远程仓库已成功配置并授权
```

## Troubleshooting

### Configuration Not Working

If automatic configuration fails, check:

1. **Token Permissions**: Ensure your GitHub token has the correct permissions
2. **Repository Access**: Verify the repository exists and your token can access it
3. **Network Connectivity**: Ensure SillyTavern can reach GitHub
4. **URL Format**: Use HTTPS format: `https://github.com/username/repo.git`

### Console Error Messages

- `缺少必要的配置项`: One or more required fields (repo_url, github_token, branch) are missing
- `Git仓库初始化失败`: Issue with local Git setup
- `远程仓库配置失败`: Problem configuring the remote repository
- `无法连接到远程仓库`: Network, permission, or authentication issue

### Manual Override

If automatic configuration isn't working, you can still use the manual UI configuration:

1. Open the plugin UI
2. Enter your repository details manually
3. Click "Configure" then "Authorize"

## Security Notes

- Keep your `config.json` file secure and don't share it
- Your GitHub token provides access to your repositories
- Consider using a dedicated repository for cloud saves
- Regularly rotate your GitHub tokens for security

## Example config.json

```json
{
  "repo_url": "https://github.com/myusername/sillytavern-saves.git",
  "github_token": "ghp_1234567890abcdef1234567890abcdef12345678",
  "branch": "main",
  "display_name": "My Cloud Saves",
  "is_authorized": false,
  "username": "",
  "last_save": null,
  "current_save": null,
  "has_temp_stash": false,
  "autoSaveEnabled": true,
  "autoSaveInterval": 15,
  "autoSaveTargetTag": ""
}
```

This will automatically configure the plugin to use your GitHub repository without any manual UI interaction. 
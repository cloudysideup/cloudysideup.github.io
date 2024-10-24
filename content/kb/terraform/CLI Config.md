```bash
export TF_CLI_CONFIG_FILE="$HOME/.terraformrc"
```

```hcl
# .$HOME/.terraformrc
credentials "app.terraform.io" {
	token = "<user_or_team_api_token>"
}
```


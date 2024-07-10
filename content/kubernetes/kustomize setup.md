```shell
curl -s "https://raw.githubusercontent.com/\
kubernetes-sigs/kustomize/master/hack/install_kustomize.sh" | zsh
mv kustomize /usr/local/bin/
kustomize
```

nvim $HOME/.zshrc:
```vim
alias kustom="kustomize"
```

source $HOME/.zshrc

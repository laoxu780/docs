# ssh

## windows生成ssh密钥对

无论您使用命令提示符还是 Windows 终端，请键入并按 Enter。这将自动生成 SSH 密钥。在我们对 Windows 11 的测试中，它创建了一个 2048 位 RSA 密钥。如果您想使用不同的算法（例如，GitHub 推荐使用 Ed25519），那么您可以键入 .

ssh-keygen

ssh-keygen -t ed25519
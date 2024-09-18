## Steps to reproduce:

1. **Clone the repository**:

```bash
git clone git@github.com:jeffzi/luals-example.git
```

2. **Initialize submodules**:

```bash
git submodule update --init --recursive
```

3. **Run linting with `lua-language-server`**:  
   Running the linting command on the workspace returns two errors related to `luassert`, despite the configuration disabling library checks. However, in VS Code, no errors are displayed.

`luarc.json`:

```json
{
  "workspace.library": ["annotations"],
  "diagnostics.libraryFiles": "Disable"
}
```

```bash
lua-language-server --check . --logpath logs
# Diagnosis complete, 2 problems found, see logs/check.json
```

Passing the full path doesn't work either.

```bash
lua-language-server --check /Users/jeffzi/Projects/personal/lua/luals-example/ --logpath logs
# Diagnosis complete, 2 problems found, see logs/check.json
```

4. **Modify the `workspace.library` path**:  
   Edit the `"workspace.library"` field in `luarc.json` to use an absolute path instead of a relative one, for example:

```json
 "workspace.library": ["/Users/jeffzi/Projects/personal/lua/luals-example/annotations"]
```

Then, run the linting command again:

```bash
lua-language-server --check . --logpath logs
# Diagnosis complete, no problems found
```

In VS Code, no errors are displayed as well.

5. **Testing with an absolute path in the command-line**:  
   Running the `lua-language-server` command with the full workspace path will still result in errors:

```bash
lua-language-server --check /Users/jeffzi/Projects/personal/lua/luals-example/ --logpath logs
# Diagnosis complete, 2 problems found, see logs/check.json
```

## Conclusion

The documentation of [--check](https://luals.github.io/wiki/usage/#--check) doesn't specify whether the path must be full or relative, but it appears that the only combination that works — if we want to avoid checking library files stored within the workspace — is to pass a relative path to `--check` and a full path to `"workspace.library"`.

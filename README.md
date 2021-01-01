# Vault kv renamer tool

**Author: Mick Miller**

**Date: 2020-12-16**

___

This bash script was built primarily to rename a "root" path for  vault key values. For example if you have a path "kv/cnn" and you want to rename the path to "kv/cnn00", this is the tool to accomplish that task.

## Contents

1. The config_rename.json file
2. Installation instructions
3. Running the script
4. Acknowledgements

A couple of notes before diving in: 

* Back up your source secrets engine(s). 
* Understand what this script is doing before you run it and tweak as needed.
* You will need to configure the config.json file for your specific use case.

> NOTE: You may notice the pattern `# echo "DEBUG ${LINENO}: "Some string"` in the script. This is used for debugging the script. I left them in for you in case you wanted to trace the code; sorry if it irritates you.

___

## 1. The config_rename.json file

This configuration file is used to reduce the amount of command line arguments and limit the arguments to:

* The path to find the secrets; and
* Not storing the tokens in the configurations or code.

config.json

```
{ 
    "type_val": "kv",
    "src_url": "https://old_vault.example.com",
    "dest_url": "https://new_vault.example.com",
    "tmp_file": "./tmp.json",
}
```

### Explanation of keys and values

| Key           | Value                                                                           |
| ---           | -----                                                                           |
| `type_value`  | The type of secrets engine in the source vault instance                         |
| `src_url`     | The source vault instance URL                                                   |
| `dest_url`    | the destination vault instance URL                                              |
| `tmp_file`    | The name of the output temp JSON file; you should not need to change this value |

## 2. Installation instructions

The code assumes that both the Hashi Vault client and jq are installed before you start and tests for the presence of both.

### Installing Vault and jq
If you are using the Homebrew package manager on mac OS, run the following:

```
 $ brew install jq
 $ brew install vault
```

This script has not been tested on Windows or Linux, only macOS. I will test Ubuntu at some point and refactor as needed. This "*should*" work to rename and copy across vault instances.  Like a move and rename.  I have not tested that yet.

### Installing the script
```
# Clone the repo and then change to the directory.
$ git clone <this repo url>
$ cd vault_kv_renamer

```

---

## 3. Running the script
Command line arguments description

```
 echo "You typed:" "${0}" -s "${SRC_TOKEN}" -d "${DEST_TOKEN}" -p "${VAULT_PATH_OLD}" -o "${OLD_ROOT}" -n "${NEW_ROOT}"
  echo
  echo "Usage: "
  echo "format : " "${0} " -s {source token} -d {destination token} -p {path} -o {old_root} -n {new_root}
  echo "example: " "${0} " "-s xxxxxxx -d xxxxxxx -p /secret/cnn/" -o kv/cnn/ -n kv/cnn00/
  echo "Note: trailing slash in path is important"

$ vault_kv_renamer.sh \
  -s "${SRC_TOKEN}" \
  -d "${DEST_TOKEN}" \
  -p "${VAULT_PATH_OLD}" \
  -o "${OLD_ROOT}" \
  -n "${NEW_ROOT}"

Usage:
  Format : ./vault_kv_migrator.sh \ 
    -s {source token} \
    -d {destination token} \
    -p {path} 
    -o {old_root} 
    -n {new_root}
  
  Example: ./vault_kv_migrator.sh \
    -s xxxxxxx \
    -d xxxxxxx \ 
    -p /secret/cnn/" \ 
    -o kv/cnn/ \
    -n kv/cnn00/ \
  
  Note   : A trailing slash in path is required."
```  

---

## 4. Acknowledgements
Many thanks to the following folks:

* agaudreault-jive (https://github.com/hashicorp/vault/issues/5275)
* user2599522 (https://stackoverflow.com/a/61000422)
* kir4h (https://github.com/kir4h/rvault)

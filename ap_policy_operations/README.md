# README

## ap_policy_operations.py

### Description

A modified version of policy_operations.py from the CBAPI examples. This version has additional arguments added to support the bulk migration of policies between PSC instances.

### How to List, Export and Import Policies

#### How to list all policies

To list all policies in CSV format run the following command.

```shell
python3 ap_policy_operations.py --profile default list
```
**Sample Output**

```
"Policy id","Name","Description","Priority"
"109295","Advanced","Strict prevention, not recommended as a starting policy. Thoroughly test in your environment before moving endpoints into this policy.","MEDIUM"
"109293","Monitored","No prevention. Use to detect activity before moving endpoints into a prevention policy, or for mission critical endpoints.","MEDIUM"
"109294","Standard","Prevents known malware and reduces false positives. Used as the default policy for all new sensors, unless sensor group criteria is met.","MEDIUM"
```
To list all policies and associated rules, run the list command with the -V or --verbose switch

```shell
python3 ap_policy_operations.py --profile default list -V
```

**Sample Output**
```csv
Policy id 109295: Advanced (Strict prevention, not recommended as a starting policy. Thoroughly test in your environment before moving endpoints into this policy.)
Rules:
  153: TERMINATE when REPUTATION KNOWN_MALWARE is RUN
  154: TERMINATE when REPUTATION COMPANY_BLACK_LIST is RUN
  155: TERMINATE when REPUTATION SUSPECT_MALWARE is RUN
  156: TERMINATE when REPUTATION PUP is RUN
```

#### How to export a policy by Name or Policy ID

After identifying the policy you want to export with the list command command, you can use the export command to save the policy to a JSON file. The json file name will be saved as policy_<policy_id>.json. 

```shell
python3 policy_operations.py --profile default export -N MacOS
```
```shell
python3 policy_operations.py --profile default export -i 117395
```
**Sample Output**
```shell
Wrote policy 117395 MacOS to file policy-117395.json
```
*Note*
> - It is recommended not to change the filename as the exported JSON does not include the Policy ID, Name or Description of the exported policy.
> - When using the -N switch to search by policy name, the search argument is case sensitive.


#### How to import a policy with the import command

To import a policy from an exported JSON policy file , run the following command. This command requires four parameters:

 - -N, Policy Name
 - -d, Policy descriptiom
 - -p, Priority level, LOW, MEDIUM or HIGH
 - -f, JSON policy file

```shell
python3 policy_operations.py --profile default import -N MacOSV2 -d "MacOS Policy Version 2" -p MEDIUM -f policy-117395.json
```
**Sample Output**
```shell
Added policy. New policy ID is 133354
```

#### How to create policy import commands with the generator command.

The generator command allows you to search for a policy by name or by policy id and then generate a import command that specifies each required argument.

```shell
python3 policy_operations.py --profile default generator -N MacOS
```
**Sample Output**
```shell
policy_operations.py --profile <<<CHANGEME>>> import -N "MacOS" -d "MacOS specific policy." -p MEDIUM -f policy-117395.json
```

*Note*
> If a policy name or policy id is not provided, the generator command will generate an import command for all policies.


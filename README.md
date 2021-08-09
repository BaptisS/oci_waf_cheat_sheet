# oci_waf_cheat_sheet



# Enable oci_waf_threat-feeds

1.1.2- Copy and Paste (CTRL+SHIFT+’V’) the command below in your Cloud Shell session : 

(Replace ‘ocid1.waaspolicy.oc1..aaaaaaaaxxxxxxxxxxx’ by your WAF Policy OCID - copied in the previous step.)


```
export wafpolid=ocid1.waaspolicy.oc1..aaaaaaaaxxxxxxxxxxx

oci waas threat-feed list --waas-policy-id $wafpolid --all | jq -r '.[]'>/tmp/threat-feeds.json
sed -i 's/OFF/BLOCK/g' /tmp/threat-feeds.json
sed -i '$d' /tmp/threat-feeds.json
oci waas threat-feed update --waas-policy-id $wafpolid --threat-feeds file:///tmp/threat-feeds.json
```

# Backup/Restore oci_waf_threat-feeds

1.1.2- Copy and Paste (CTRL+SHIFT+’V’) the command below in your Cloud Shell session : 

(Replace ‘ocid1.waaspolicy.oc1..aaaaaaaaxxxxxxxxxxx’ by your WAF Policy OCID - copied in the previous step.)

Backup : 

```
export wafpolid=ocid1.waaspolicy.oc1..aaaaaaaaxxxxxxxxxxx


oci waas threat-feed list --waas-policy-id $wafpolid --all | jq -r '.[]'>/tmp/threat-feeds_backup.json
sed -i '$d' /tmp/threat-feeds_backup.json

```
Restore : 
```
export wafpolid=ocid1.waaspolicy.oc1..aaaaaaaaxxxxxxxxxxx


oci waas threat-feed update --waas-policy-id $wafpolid --threat-feeds file:///tmp/threat-feeds_backup.json
```


# Backup/Restore oci_waf_protection rules

1.1.2- Copy and Paste (CTRL+SHIFT+’V’) the command below in your Cloud Shell session : 

(Replace ‘ocid1.waaspolicy.oc1..aaaaaaaaxxxxxxxxxxx’ by your WAF Policy OCID - copied in the previous step.)

Backup : 

```
export wafpolid=ocid1.waaspolicy.oc1..aaaaaaaaxxxxxxxxxxx


oci waas protection-rule list --waas-policy-id $wafpolid --all --output json --query "data [].{key:\"key\",action:\"action\",name:\"name\",description:\"description\"}" > backup_protectionrules.json
```

Restore : 

```
export wafpolid=ocid1.waaspolicy.oc1..aaaaaaaaxxxxxxxxxxx


oci waas waf-config update --waas-policy-id $wafpolid --protection-rules file://backup_protectionrules.json
```


# Backup/Restore oci_access_control_rules

1.1.2- Copy and Paste (CTRL+SHIFT+’V’) the command below in your Cloud Shell session : 

(Replace ‘ocid1.waaspolicy.oc1..aaaaaaaaxxxxxxxxxxx’ by your WAF Policy OCID - copied in the previous step.)

Backup : 

```
export wafpolid=ocid1.waaspolicy.oc1..aaaaaaaaxxxxxxxxxxx


oci waas access-rule list --waas-policy-id $wafpolid --all --output json > access_rule_export.json
cat access_rule_export.json | jq .data > access_rules_backup.json

```

Restore : 

```
export wafpolid=ocid1.waaspolicy.oc1..aaaaaaaaxxxxxxxxxxx


oci waas waf-config update --waas-policy-id $wafpolid --access-rules file://access_rules_backup.json

```


# List oci_waf_protection rules

1.1.2- Copy and Paste (CTRL+SHIFT+’V’) the command below in your Cloud Shell session : 

(Replace ‘ocid1.waaspolicy.oc1..aaaaaaaaxxxxxxxxxxx’ by your WAF Policy OCID - copied in the previous step.)


```
export wafpolid=ocid1.waaspolicy.oc1..aaaaaaaaxxxxxxxxxxx


oci waas protection-rule list --waas-policy-id $wafpolid --all --output table --query "data [?contains(\"labels\",'A1')].{WAFKey:\"key\",Labels:\"labels\"}"
```


Top OWASP 10 vulnerability groups include:

• A1 – Injections (SQL, LDAP, OS, etc.)

• A2 – Broken Authentication and Session Management

• A3 – Cross-site Scripting (XSS)

• A4 – Insecure Direct Object References

• A6 – Sensitive Data Exposure

• A7 – Missing Function-Level Access Control 




# Update oci_waf_protection rules

1.1.2- Copy and Paste (CTRL+SHIFT+’V’) the command below in your Cloud Shell session : 

(Replace ‘ocid1.waaspolicy.oc1..aaaaaaaaxxxxxxxxxxx’ by your WAF Policy OCID - copied in the previous step.)


```
export wafpolid=ocid1.waaspolicy.oc1..aaaaaaaaxxxxxxxxxxx

rm -f protectionrules.json
oci waas protection-rule list --waas-policy-id $wafpolid --all --output json --query "data [?contains(\"labels\",'Recommended')].{key:\"key\",action:'DETECT',name:\"name\",description:\"description\"}" > protectionrules.json

oci waas waf-config update --waas-policy-id $wafpolid --protection-rules file://protectionrules.json
```


Sample Protection Rules List : protectionrules.json
 

# List All WAF Policies in a Tenant (All compartments)

1.1.2- Copy and Paste (CTRL+SHIFT+’V’) the command below in your Cloud Shell session : 


```
wafpolist=$(oci search resource structured-search --query-text "query waaspolicy resources" --output json --query "data.items [*].{OCID:identifier}" | jq -r '.[] | .OCID')

for polid in $wafpolist;do oci waas waas-policy get --waas-policy-id $polid --output table --query "data.{OCID:id,PolicyName:\"display-name\",Origins:origins}" ; done
```


 


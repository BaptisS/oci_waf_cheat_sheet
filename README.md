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
 



# Detection: User Added to Sudo Group

## Objective

Detect when a local Linux account is added to the `sudo` group.

Adding an account to the sudo group grants the account the ability to
execute commands with elevated privileges. This activity should
therefore be monitored in a SOC environment.

## Lab Environment

-   Target: Ubuntu Server
-   SIEM: Wazuh
-   Log source: `/var/log/auth.log`
-   Test account: `soclab-test`

## Simulation

A test account was added to the sudo group:

``` bash
sudo usermod -aG sudo soclab-test
```

The change was verified using:

``` bash
getent group sudo
id soclab-test
```

## Linux Log Evidence

Ubuntu recorded the privilege modification in `/var/log/auth.log`:

``` text
usermod: add 'soclab-test' to group 'sudo'
usermod: add 'soclab-test' to shadow group 'sudo'
```

## Detection Gap

The default Wazuh configuration recorded the sudo command using Rule
5402:

-   Description: `Successful sudo to ROOT executed.`
-   Level: 3

However, this alert did not clearly identify that an account had been
added to the privileged `sudo` group.

A custom detection rule was therefore created.

## Custom Wazuh Rule

``` xml
<group name="local,syslog,account_change,">

  <rule id="100100" level="10">
    <program_name>usermod</program_name>
    <match>to group 'sudo'</match>
    <description>Privileged account change: user added to sudo group.</description>
    <mitre>
      <id>T1098</id>
    </mitre>
  </rule>

</group>
```

## Rule Testing

The rule was tested using:

``` bash
sudo /var/ossec/bin/wazuh-logtest
```

Test event:

``` text
Aug 09 07:10:09 soc-ubuntu usermod[94538]: add 'soclab-test' to group 'sudo'
```

Wazuh successfully matched:

-   Rule ID: `100100`
-   Level: `10`
-   MITRE ATT&CK: `T1098`
-   Tactic: Persistence
-   Technique: Account Manipulation

## Validation

The account was removed from the sudo group and then added again to
generate a real event.

Wazuh successfully generated:

> Privileged account change: user added to sudo group.

This confirmed that the custom rule successfully detects the simulated
privileged account modification.

## Detection Outcome

**Status: Detected**

The test demonstrated the full detection-engineering process:

1.  Generate controlled activity
2.  Inspect host logs
3.  Review existing SIEM detection
4.  Identify a detection gap
5.  Develop a custom detection
6.  Test the rule
7.  Reproduce the activity
8.  Validate the alert in Wazuh

## MITRE ATT&CK

The activity was mapped to:

**T1098 --- Account Manipulation**

An attacker who has obtained sufficient privileges may modify accounts
or group memberships to maintain or expand privileged access.

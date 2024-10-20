# Diagnostics

If you have encountered any issues while installing Evernode, Check if your error is one of the following.

_Note: If any of the following doesn't help in your situation please send it to Evernode support team as a GitHub issue [here](https://github.com/EvernodeXRPL/evernode-host/issues)._

## 1. Keeps on retry loop
- Abort and try again
  - This won't rollback your installation. The next try will resume from where you've aborted.
- Rolling back will clear installation files but won't deregister your account. You'll have to use [deregister command](./maintenance.md#deregister-a-host) to deregister from Evernode.
- If this doesn't help check for the installation error log for the error and send it to Evernode support team as a GitHub issue [here](https://github.com/EvernodeXRPL/evernode-host/issues).

## 3. Asking to re-check the balance
- If the installer is asking `Do you want to re-check the balance`, when you are installing or transferring to a VM.
- This means you do not have enough balance in your Host Xahau account to install and maintain a host.
- Please check your Host Xahau account whether it has following XAH and EVR balances. If not, fund your account and retry.
  - You should have minimum XAH balance in your account to maintain your host for 3 months.
    - This is to pay your upcoming heartbeats, reserves etc, It won't be expend at the installation.
    - The value is at around `18 XAH` when this documentation is last updated.
  - You should have minimum EVR balance in your account to do **register (on fresh install)** or **re-register (on transfer install)** transactions.
    - If you are doing a register (on fresh install) - You should have `<Registration Fee> EVRs` in your account (`500 EVRs` when this documentation is last updated).
    - If you are doing a re-register (on transfer install) - You should have `0.00000001 EVRs` in your account.

## 4. CLI_OUT: INVALID_REG error on installation
- If your installation is failing with `CLI_OUT: INVALID_REG`.
- This means your host is already registered in Evernode but now you are trying to install again with different host specifications.
- So you can [transfer](maintenance.md#transfer-the-host-registration) your registration and re-install again.
- **Not recommended:** You can also uninstall/deregister and install again. But it will only refund you half of registration fee, if you have already been in Evernode network. So we recommend not to do this if you are not intending to permanently leave the network.

## 5. Uninstall failure
- If your uninstall process failed at any point you can abort it and retry again.
- Deregistration failure at Uninstall
  - If your uninstall process failed at deregistration, you will be prompted for retry or you can abort uninstallation and retry later
  - Or it would prompt you to skip deregistering and continue uninstall.
    - This would clean up Evernode binaries from your machine but registration will be preserved
    - You'll have to use [deregister command](./maintenance.md#deregister-a-host) to deregister from Evernode.
      
## 6. Transfer failure
- In the normal transfer process, If you have previous failed/partial registration, It would first complete the registration and then start the transfer.
- If your transfer process fails at any point aborting it and retrying would start the process from the point of failure.
  
## 7. Deregistration failure
- In the normal deregister process, If you have previous failed/partial registration, It would complete the registration and then start the deregister.
- If your deregister process fails at any point aborting it and retrying would start the process from the point of failure.
      
## 8. Evernode update failure
- Evernode update wouldn't remove any of your binaries or data. It would only replace them with the new version.
- If your update failed when you are trying to update Evernode you can abort it and start again from the failure.
- **Do not deregister or uninstall.** Even if your update failed your registration with Evernode will remain intact.
- If you are still encountering problems with the update, you can transfer your previous registration and re-install.
- You can find how to transfer your host from [here](maintenance.md#transfer-the-host-registration).

## 9. Failing to fetch the Sashimono version
```
jq: error (at <stdin>:1): Cannot iterate over null (null)
Sashimono null not found.
```
or
```
Failed to retrieve latest version data.
```
- If you are getting above error when running the Evernode commands.
- This could mean you got a connection issue, or your IP is blocked by Github due to exceeding request limitations.
- Wait for some time and try again, Your limitation will be lifted and you'll be able to successfully execute the command again.

## 10. No offered lease after installation
- Not the Evernode installation will only mint the leases, It won't create lease offers for you.
- You have to run `evernode offerlease` to offer the minted leases.

## 11. No rewards even if the Host is active
- **Case 1:** In the version 0.8.2 you won't rewards if your host has unoffered leases.
- **Case 2:** Your reputation will be set to 0, if your host has less than 3 instances.
- **Case 3:** Your reputation will be set to 0, if your host's lease fee is more than `(reward distribution for the moment / host count) * 110%`
- **Case 4:** Your reputation will be set to 0, if you haven't upgraded to v0.8.2 within **one week**.
- **Case 5:** Your reputation will be set to 0, if your machine is from on of the **Sanctioned Entities** mentioned in [Evernode license](https://raw.githubusercontent.com/EvernodeXRPL/evernode-test-resources/main/license/evernode-license.pdf).
  
## 12. Lease offer creation failure
- **Case 1:** If your lease offering failed when you run `evernode offerlease`, Executing the command again would offer the remaining leases.
- **Case 2:** If `evernode status` shows unoffered lease, but still you are getting following error when executing `evernode offerlease`.
  - `evernode status` output :
    ```
    Available Lease offers: 3 out of 6
    ````
  - `evernode offerlease` output :
    ```
    CLI_OUT: LEASE_OFFER_ERR
    MB_CLI_EXITED
    There was an error in creating lease offers
    ```
  - Try configuring instance counts again
    - Command: `evernode config resources <ram> <swap> <disk> <instance count>`
    - Arguments are optional, You can pass `0` if you don't want to change
    - Example:
      - Run `evernode config resources 0 0 0 5` it'll fix your instances.
      - Then if you want, you can change it back to 6 by running `evernode config resources 0 0 0 6`
    
## 13. Stuck installation
- If your installation is stuck on `Checking server wss:\\....` this could indicate you have some broken package dependencies.
- Try removing `rm -r /tmp/evernode-setup-helpers` and install Evernode again.

## 14. Re-config failure
- If `evernode config` command failed in any reconfiguration, retry executing it again.
- If retry doesn't help change the value back to the original using the `evernode config` and change back to the new value again using the same command.

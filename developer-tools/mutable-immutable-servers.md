# Mutable vs Immutable Servers

## Mutable Servers

**Mutable servers** refer to ones where the server's configuration can change, such as the OS, installed packages, or even the deployed application itself.

### Pros
- No overhead of managing virtual machines
- Ad hoc commands (i.e. security patches) are really simple

### Cons
- If a deployment fails, server can be left in a broken state
- Because changes are done over time, you are not starting with a _known_ working configuration each time you deploy
- Any changes to the server need to be tested separately to ensure nothing breaks (i.e. removing or upgrading a package)

## Immutable Servers

**Immutable servers** refer to the exact opposite, and more specifically in the context of virtualized machines.

Immutable servers are best with virtualized machines; you don't want to restart an entire physical host, or even "get a new physical host". With virtualization, you can easily start a brand new virtual guest.

### Pros
- Having server in a known working state for each deployment; higher trust and confidence
- Can use deployment and scaling features available with cloud platforms, such as auto scaling
	- Even manual scaling is easy once server images are created
- If deployments fail, you can easily revert back to the previous server image
- No additional testing required for system level changes

### Cons
- Build times are longer since you are creating server images
- Changes to the OS require a bake and re-deploy, which can be time-consuming

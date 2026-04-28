# Basics

## Reliability

Continuing to work correctly, even when things go wrong.

- The things that can go wrong are called **faults**.
- A **fault** is usually defined as one component of the system deviating from its spec.
- A **failure** is when the system as a whole stops providing the required service to the user.
- Systems that anticipate faults and can cope with them are called **fault-tolerant** or **resilient**.

## Hardware faults

Hard disks crash, RAM becomes faulty, the power grid has a blackout, someone unplugs the wrong network cable.

**Solution:** add redundancy to individual hardware components.

- **Disks** — set up in a RAID configuration.
  - **RAID** (Redundant Array of Independent Disks): a way to combine multiple physical disks so they act as one unit, with built-in redundancy.
- **Servers** — dual power supplies.
- **Hot-swappable CPUs** — "hot-swap" means you can remove and replace a component while the system is still running (no shutdown, no downtime).
- **Data centers** — batteries and diesel generators for backup power.

# xyz.openbmc_project.RDE

## Overview

This module defines D-Bus interfaces for Redfish Device Enablement (RDE)
services, allowing the BMC to discover, manage, and interact with
Redfish-capable devices over supported transports such as PLDM, NVMe-MI.

It provides two key interfaces:

- `xyz.openbmc_project.RDE.Device`: Represents each RDE-capable endpoint.

- `xyz.openbmc_project.RDE.Manager`: Centralized interface for invoking Redfish
  operations and querying device metadata.

- `xyz.openbmc_project.RDE.OperationTask`: Tracks the status and lifecycle of an
  asynchronous Redfish operation.

---

## Components

### 1. RDE Device Interface

The `Device` interface exposes:

- Supported Redfish operations (e.g., GET, REPLACE)
- Protocol-level feature support (e.g., BEJ encoding, Redfish Events)
- Redfish schema descriptors used for client introspection
- Device-wide metadata including unique identifier and negotiation status

This interface is automatically instantiated for each discovered RDE device.

### 2. RDE Manager Interface

The `Manager` interface provides:

- Methods to perform Redfish operations on any device by ID
- Access to schema metadata and supported capabilities
- Support for inline or file-based payloads and responses
- A simplified abstraction for clients that do not directly track device paths
- Retrieve Redfish-compatible schema details for a specified device, including
  resource URIs, schema types, and available operations.

### 3. RDE OperationTask Interface

The `OperationTask` interface provides:

- Lifecycle tracking for Redfish operations including status, completion codes,
  and cancellation
- D-Bus signals to notify clients about progress or state changes
- Enum-based status and result codes aligned with Redfish and PLDM conventions
- Useful for asynchronous workflows where execution may span multiple phases or
  rely on external device responses

This interface is typically instantiated per Redfish operation initiated by the
`Manager`, allowing clients to track status independently.

---

## Usage Summary

- Use the `Device` interface when working directly with a known D-Bus object
  path or to subscribe to property changes.
- Use the `Manager` interface for centralized access when you have only a device
  ID or when performing file-based or inline Redfish operations.
- Use the `OperationTask` interface when tracking or managing asynchronous
  Redfish requests.

---

## Documentation References

- `Device.interface.yaml`: YAML definition for the `Device` interface
- `Manager.interface.yaml`: YAML definition for the `Manager` interface
- `OperationTask.interface.yaml`: YAML definition for the `OperationTask`
  interface
- Enumerations include operation types, feature types, schema keys, and
  negotiation status

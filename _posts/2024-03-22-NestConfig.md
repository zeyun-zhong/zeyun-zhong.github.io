---
layout: post
toc:
  sidebar: left
title: Introducing NestConfig
---

## TL;DR
NestConfig is a Python library designed to simplify configuration management 
in applications, leveraging the power of dataclasses. It enhances your 
development experience by offering:

- **Type-Safe Configuration Updates**
- **Command-Line Integration**
- **Export Configurations to YAML**
- **Auto-Completion Support**

## Introduction

Configuring Python applications can often be a hassle, especially when 
dealing with complex hierarchies or external configuration files. 
NestConfig aims to simplify this process, providing a robust and intuitive 
approach to handling configurations.

## How NestConfig Simplifies Configuration Management

NestConfig builds upon the simplicity of dataclasses and enhances them 
with powerful merging capabilities. Let's dive into two of its core 
features: `merge_updates` and `merge_opts`.

### The `merge_updates` Method

The `merge_updates` method is a key feature of NestConfig that facilitates
the merging of configurations from external sources. Consider a scenario 
where you have a YAML file with configuration parameters you want to 
apply to your application dynamically. Here's an example `config.yaml` file content:

```yaml
LEARNING_RATE: 0.001
BATCH_SIZE: 64
MODEL:
  N_LAYER: 6
```

This file defines several configuration parameters, including a nested 
parameter under `MODEL`. To incorporate these updates into your 
application, you would typically load the YAML file and then call 
`merge_updates` on your configuration object:

```python
from nestconfig import NestConfig
from dataclasses import dataclass, field
import yaml

@dataclass
class ModelConfig:
    N_LAYER: int = 2

@dataclass
class Config(NestConfig):
    LEARNING_RATE: float = 0.001
    BATCH_SIZE: int = 64
    MODEL: ModelConfig = field(default_factory=ModelConfig)

# Load configuration updates from an external YAML file
with open('config.yaml', 'r') as file:
    config_updates = yaml.safe_load(file)

# Create a Config instance and merge updates
config = Config()
config.merge_updates(config_updates)

print(config.LEARNING_RATE)  # Output: 0.001
print(config.BATCH_SIZE)     # Output: 64
print(config.MODEL.N_LAYER)  # Output: 6
```

By executing this script, NestConfig reads the `config.yaml` file, 
loads the settings, and updates the config object. The output confirms 
that the default value of `N_LAYER` is overridden by the value specified 
in the YAML file.

#### `merge_yaml`
For convenience, NestConfig also provides a method named `merge_yaml` 
which directly loads and merges the YAML configuration file:

```python
config = Config()
config.merge_yaml('config.yaml')
print(config.MODEL.N_LAYER)  # Output: 6
```

### The `merge_opts` Method
`merge_opts` is designed to work seamlessly with command-line arguments, enabling 
easy integration with tools like argparse. This method merges a list of options 
directly into the configuration object.

Here's a simple demonstration of how to use `merge_opts` with `argparse`:

```python
from nestconfig import NestConfig
from dataclasses import dataclass, field
import argparse

@dataclass
class ModelConfig:
    N_LAYER: int = 2

@dataclass
class Config(NestConfig):
    LEARNING_RATE: float = 0.001
    BATCH_SIZE: int = 64
    MODEL: ModelConfig = field(default_factory=ModelConfig)

# Set up argparse
parser = argparse.ArgumentParser(description='Test merge_opts function.')
parser.add_argument(
    "--opts",
    help="See the Config definition for all options",
    default=["MODEL.N_LAYER", "12"],
    nargs=argparse.REMAINDER,
)

args = parser.parse_args()

# Create a Config instance and merge command-line options
config = Config()
config.merge_opts(args.opts)

print(config.MODEL.N_LAYER)  # 12
```

In this code snippet, we set up argparse to accept command-line options. 
We then pass these options as a list to the merge_opts function, 
which merges them into our Config object. The merge_opts method ensures that 
the command-line options take precedence, updating the configuration instance's 
values accordingly.

### Saving Configurations with `to_yaml`

You can also export current configurations for reuse or inspection. 
NestConfig addresses this by providing a `to_yaml` method, enabling you to 
serialize and save your configuration objects back to YAML format. 

To demonstrate the `to_yaml` functionality, let's consider an example where we 
modify our application's configuration via command-line arguments and then 
save the resulting configuration to a YAML file:

```python
from nestconfig import NestConfig
from dataclasses import dataclass, field
import argparse
import yaml  # Ensure you have PyYAML installed

@dataclass
class ModelConfig:
    N_LAYER: int = 2

@dataclass
class Config(NestConfig):
    LEARNING_RATE: float = 0.001
    BATCH_SIZE: int = 64
    MODEL: ModelConfig = field(default_factory=ModelConfig)

# Set up argparse
parser = argparse.ArgumentParser(description='Test merge_opts function.')
parser.add_argument(
    "--opts",
    help="See the Config definition for all options",
    default=["MODEL.N_LAYER", "12"],
    nargs=argparse.REMAINDER,
)

args = parser.parse_args()

# Create a Config instance and merge command-line options
config = Config()
config.merge_opts(args.opts)

# Save the current Config as a YAML file to inspect later on
config.to_yaml("config_new.yaml")
```

In this example, after merging the command-line options into our `Config` object, 
we call `config.to_yaml("config_new.yaml")` to save the current state of the 
configuration to a file named `config_new.yaml`. This YAML file can then be 
used as a configuration template for future runs or shared among team members 
to ensure consistent settings.

### Advantages Over argparse and fvcore
While argparse is a versatile module for command-line argument parsing, 
NestConfig extends its functionality within the application's configuration context. 
With NestConfig, you can:

- Maintain Type Safety: NestConfig respects the types defined in your dataclasses, ensuring that merged values adhere to the expected types, reducing runtime errors.
- Support Nested Configurations: Unlike argparse, which typically handles flat argument structures, NestConfig can elegantly manage complex, nested configurations.
- Integrate with External Configuration Files: NestConfig can easily merge configurations from files in formats like YAML, making it versatile for projects that require external configuration files.
- Seamless argparse Integration: merge_opts can directly handle the output of argparse, allowing you to merge command-line arguments into your application's configuration object with ease.
- Comparison to fvcore: While fvcore provides a CfgNode object for managing hierarchies of configurations, NestConfig offers the added benefit of Pythonic type safety and autocomplete features due to its dataclass foundation.

## Install via pip
Code in this blog can be found in the [Project Page](https://github.com/zeyun-zhong/NestConfig).
To install NestConfig, simply use pip:
```bash
pip install nestconfig
```

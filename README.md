

https://github.com/AvitalTamir/cyphernetes/assets/83203533/604c29e9-3e02-4f6b-8f03-b9d579db9e14


<table style="border-collapse: collapse; border: none">
  <tr>
    <td style="border: none" width="256">
      <img src="./logo.png" alt="Cyphernetes Logo" width="256">
    </td>
    <td style="border: none; padding-left: 20px">
      <h1>Cyphernetes</h1>
      <p>Cyphernetes is a command-line interface (CLI) tool designed to manage Kubernetes resources using a query language inspired by Cypher, the query language of Neo4j. It provides a more intuitive way to interact with Kubernetes clusters, allowing users to express complex operations as graph-like queries.</p>
    </td>
  </tr>
</table>

## Why Cyphernetes?

Kubernetes management often involves dealing with complex and verbose command-line instructions. Cyphernetes simplifies this complexity by introducing a declarative query language that can express these instructions in a more readable and concise form. By leveraging a query language similar to Cypher, users can efficiently perform CRUD operations on Kubernetes resources, visualize resource connections, and manage their Kubernetes clusters with greater ease and flexibility.

The project is at it's earliest milestone and supports performing GET operations.
The Cypher-like grammar implementation is incomplete.
A high-level list of what's still missing:
* CREATE, SET, DELETE clauses
* The graph model of relationships between common Kubernetes resources is in very early stages

See the [project roadmap](https://github.com/AvitalTamir/cyphernetes/blob/main/ROADMAP.md) for more detailed information on what's still left to do.

## Usage

Cyphernetes offers two main commands: `query` and `shell`. Below are examples of how to use these commands with different types of queries.

### Query Command

The `query` command is used for running single Cyphernetes queries from the command line. 

Example usage:

```bash
$ cyphernetes query 'MATCH (d:Deployment {name: "nginx"}) RETURN d'
```

This command retrieves information about a Deployment named 'nginx'.

### Shell Command
The shell command launches an interactive shell where you can execute multiple queries in a session.

To start the shell:

```bash
$ cyphernetes shell
```
Within the shell, you can run queries interactively. Here are some examples:

### Basic Node Match

```graphql
MATCH (d:Deployment) RETURN d
```
This query lists all Deployment resources.

### Node with Properties

```graphql
MATCH (d:Deployment {app: "nginx"}) RETURN d
```
Retrieves Deployments where the app label is 'nginx'.

### Multiple Nodes

```graphql
# Multiple matches
MATCH (d:Deployment), (s:Service), (i:Ingress) RETURN d, s, i
```
Lists Deployments and their associated Services.

### Relationships

```graphql
# Match 2 or more related resources
MATCH (d:Deployment)->(p:Pod)->(s:Service) RETURN d, s, i
```
Lists Deployments, Pods that belong to these deployments, and the Service(s) that route to these pods.

### Node with Multiple Properties

```graphql
MATCH (s:Service {type: "LoadBalancer", region: "us-west"}) RETURN s
```
Finds Services of type "LoadBalancer" in the "us-west" region.

### Returning specific properties

```graphql
MATCH (s:Service) RETURN s.status.LoadBalancer
```

### Multiple matches and returns

```graphql
MATCH (d:Deployment), (s:Service) RETURN d.metadata.name, s.status.LoadBalancer
```

Remember to type exit or press Ctrl-C to leave the shell.

## Development

Cyphernetes is written in Go and utilizes a parser generated by goyacc to interpret the custom query language.

### Prerequisites

- Go (1.16 or later)
- goyacc (for generating the parser)
- Make (for running make commands)

### Getting Started

To get started with development:

1. Clone the repository:
```bash
$ git clone https://github.com/avitaltamir/cyphernetes.git
```

2. Navigate to the project directory:
```bash
$ cd cyphernetes
```

### Building the Project

Use the Makefile commands to build the project:

- Build & Test:
```bash
$ make
```

- To build the binary:
```bash
$ make build
```

- To run tests:
```bash
$ make test
```

- To generate the grammar parser:
```bash
$ make gen-parser
```

- To clean up the build:
```bash
$ make clean
```

### Contributing

Contributions are welcome! Please feel free to submit pull requests, open issues, and provide feedback.

## License

Cyphernetes is open-sourced under the MIT license. See the [LICENSE](LICENSE) file for details.

## Acknowledgments

- Thanks to the [Neo4j](https://neo4j.com/) community for the inspiration behind the query language.

## Authors

- _Initial work_ - [Avital Tamir](https://github.com/avitaltamir)

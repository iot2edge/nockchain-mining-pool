# Nockchain CUDA Miner

A high-performance GPU miner for [Nockchain](https://nockchain.com) using CUDA. Connects to a Nockchain stratum pool and mines blocks using NVIDIA GPUs.

Built with CUDA 13.0.

## Requirements

- Linux (x86_64)
- NVIDIA GPU with CUDA support
- CUDA runtime libraries installed
- A running Nockchain node with stratum support

## Usage

```bash
./nockchain-cuda-miner [options]
```

### Options

| Flag | Description | Default |
|------|-------------|---------|
| `--user USER` | Miner user identifier | `miner1` |
| `--device N` | CUDA device index, or `all` for all GPUs | `all` |
| `--constraint-dir DIR` | Path to constraint JAM files | auto-detect |
| `--submit-candidate` | Use `mining.submit_candidate` (server generates proof) | on |
| `--submit-proof` | Use `mining.submit` with full CUDA proof | off |
| `--debug-pow-hash` | Print first 7 proof object hashes and PoW digest each proof | off |
| `--workers N` | Number of CPU threads for proving (hybrid mode only) | auto |
| `--prove-once H0..H4 N0..N4 [POW_LEN] [VERSION]` | Run prover once and print hashes for comparison with Hoon | — |
| `--help` | Show help | — |

### Example

Mine on all GPUs with default settings:

```bash
./nockchain-cuda-miner
```

Mine on a specific GPU with a custom user ID:

```bash
./nockchain-cuda-miner --user myworker --device 0
```

## How It Works

The miner connects to a Nockchain node's stratum server, receives mining jobs, and uses CUDA kernels to search for valid nonces across one or more GPUs. By default it submits candidate solutions (`mining.submit_candidate`) and lets the server generate the proof, similar to a CPU miner. Alternatively, `--submit-proof` enables full on-GPU proof generation.

Hash rates are periodically reported to the pool.

## Notes

- Ensure your Nockchain node is built from the latest source and launched with `./target/release/nockchain` (stratum must support `mining.submit_candidate`).
- Set the `NOCKCHAIN_DEBUG` environment variable for additional debug output.

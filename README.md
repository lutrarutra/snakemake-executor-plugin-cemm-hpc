# snakemake-executor-plugin-cemm-hpc

An adaptation of [Generic Snakemake executor plugin](https://github.com/snakemake/snakemake-executor-plugin-cluster-generic) for submission of jobs to CeMM high performance computing cluster.

## Requires Snakemake 8

## Setup

- `git clone https://github.com/lutrarutra/snakemake-executor-plugin-cemm-hpc`
- `pip install -e ./snakemake-executor-plugin-cemm-hpc`

## Example Use

```bash
snakemake <rule> \
    --executor cemm-hpc \
    --cemm-hpc-submit-cmd "sbatch --nodes=1 --cpus-per-task={threads} --mem={resources.mem_mb} --partition={resources.queue} --qos={resources.queue} --job-name={rule} --output=LOGS/{rule}/{resources.localrule_log}.out" \
    --cemm-hpc-cancel-cmd "scancel"
```

## Example Rule

```python

rule gunzip:
    input:
        gtf_gz_annotation = config["create_star_index"]["gtf_annotation"]
    output:
        gtf_annotation = temp(config["create_star_index"]["gtf_annotation"].removesuffix(".gz"))
    threads: 4
    resources:
        mem_mb = f"{16 * 1024}",
        time = f"02:00:00",
        queue = "tinyq",
        localrule_log = "snakemake"
    shell: "gunzip --keep {input.gtf_gz_annotation}"
```


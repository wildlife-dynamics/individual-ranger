# ICF Individual Ranger Report

Ecoscope workflow that generates an individual ranger patrol report for ICF (Instituto de Conservación Forestal, Honduras).

## What you get

For a given ranger and time period, this workflow:
- Fetches patrol and event data from EarthRanger
- Calculates patrol statistics (total patrols, distance, time, events)
- Generates patrol track and event scatterplot maps
- Builds patrol and event summary tables
- Downloads event notes and attachments
- Produces a Word document report (.docx) from the ICF template

**Outputs:** Word report, PNG maps, HTML tables, dashboard with 4 stat cards + 2 tables + 2 maps.

## Configuration

Edit `param.yaml` to configure the workflow:

```yaml
er_client_name:
  data_source:
    name: mep          # EarthRanger site name

ranger_name:
  name: "Tevin Temu"   # Ranger full name as it appears in EarthRanger

time_range:
  since: "2025-01-01T00:00:00.000Z"
  until: "2026-03-12T23:59:00.000Z"
  timezone:
    label: America/Tegucigalpa
    tzCode: CST
    name: Tegucigalpa
    utc: -06:00

template_path:
  template_path: "/path/to/ICF Individual Ranger Report-3.docx"
```

The template is included in `resources/templates/`.

## Compile & Run

**Compile** (generates the Python package from `spec.yaml`):

```bash
./dev/recompile.sh
```

**Run** against a real EarthRanger connection:

```bash
export ECOSCOPE_WORKFLOWS_RESULTS="file:///tmp/ranger-report-results"
pixi run --manifest-path ecoscope-workflows-ranger-report-workflow/pixi.toml \
  ecoscope-workflows-ranger-report-workflow run --config-file param.yaml --execution-mode sequential
```

**Run a test case** using the CLI helper:

```bash
./dev/pytest-cli.sh ranger-report --case all-grouper
```

## Requirements

- [pixi](https://pixi.sh) package manager
- EarthRanger connection named in `param.yaml`

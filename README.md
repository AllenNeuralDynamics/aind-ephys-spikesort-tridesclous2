# Spike sorting with Lupin for AIND ephys pipeline
## aind-ephys-spikesort-lupin


### Description

This capsule is designed to spike sort ephys data using Tridesclous2, a spike sorting algorithm developed and optimized with SpikeInterface sorting components.

This capsule spike sorts preprocessed ephys stream and applies a minimal curation to:

- remove empty units
- remove excess spikes (falling beyond the end of the recording)


### Inputs

The `data/` folder must include the output of the [aind-ephys-preprocessing](https://github.com/AllenNeuralDynamics/aind-ephys-preprocessing), containing 
the `data/preprocessed_{recording_name}` folder.

### Parameters

The `code/run` script takes the following arguments:

```bash
  --raise-if-fails      Whether to raise an error in case of failure or continue. Default True (raise)
  --skip-motion-correction
                        Whether to skip Lupin motion correction. Default: True
  --min-drift-channels MIN_DRIFT_CHANNELS
                        Minimum number of channels to enable Lupin motion correction. Default is 96.
  --n-jobs N_JOBS       Number of jobs to use for parallel processing. Default is -1 (all available cores). 
                        It can also be a float between 0 and 1 to use a fraction of available cores
  --params-file PARAMS_FILE
                        Optional json file with parameters
  --params-str PARAMS_STR
                        Optional json string with parameters

```

A list of spike sorting parameters can be found in the `code/params.json`:

```json
{
    "job_kwargs": {
        "chunk_duration": "1s",
        "progress_bar": false
    },
    "sorter": {
        "apply_preprocessing": true,
        "apply_motion_correction": true,
        "motion_correction_preset": "dredge_fast",
        "clustering_ms_before": 0.3,
        "clustering_ms_after": 1.3,
        "whitening_radius_um": 100.0,
        "detection_radius_um": 50.0,
        "features_radius_um": 75.0,
        "template_radius_um": 100.0,
        "freq_min": 150.0,
        "freq_max": 7000.0,
        "cache_preprocessing_mode": "auto",
        "peak_sign": "neg",
        "detect_threshold": 5,
        "n_peaks_per_channel": 5000,
        "n_svd_components_per_channel": 5,
        "n_pca_features": 4,
        "clustering_recursive_depth": 3,
        "ms_before": 1.0,
        "ms_after": 2.5,
        "sparsity_threshold": 1.5,
        "template_min_snr": 2.5,
        "gather_mode": "memory",
        "job_kwargs": {},
        "seed": null,
        "save_array": true,
        "debug": false
    }
}
```

### Output

The output of this capsule is the following:

- `results/spikesorted_{recording_name}` folder, containing the spike sorted data saved by SpikeInterface and the spike sorting log
- `results/data_process_spikesorting_{recording_name}.json` file, a JSON file containing a `DataProcess` object from the [aind-data-schema](https://aind-data-schema.readthedocs.io/en/stable/) package.


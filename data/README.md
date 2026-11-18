# Data

Data of the article

> Beyond accuracy: bias and error direction in machine learning surrogates for pushover assessment of RC frames.

The database holds 73,023 planar reinforced concrete frames. Each frame was analyzed by a pushover analysis in OpenSees, and its target displacement and performance level were evaluated with the nonlinear static procedure of the Turkish Building Earthquake Code (TBEC 2018) for one site (DD-2 earthquake level) and five soil classes. *Actual* denotes a value from this analysis and *predicted* a value returned by a model of the article.

## Files

| File | Rows | Content |
|---|---|---|
| `frames.csv` | 73,023 | Inputs, capacity quantities and performance level of every frame |
| `normalized_curves.csv` | 73,011 | Normalized capacity curve of the frames with a stored curve |
| `predictions.csv` | 73,023 | Stored predictions of the models |

All files are comma-separated (UTF-8) and are joined by `frame_id`. Empty cells mean that the value does not exist for that frame.

## frames.csv

| Column | Description | Unit |
|---|---|---|
| frame_id | Identifier of the frame | - |
| sampling_wave | Sampling wave: 1 Latin hypercube sample, 2 wave enriched in CP and C frames, 3 wave enriched near the boundaries between levels | - |
| data_split | `training` (training pool, 51,116 frames) or `hold-out` (21,907 frames) | - |
| N_story | Input 1 (geometry): Number of stories | - |
| N_bay | Input 2 (geometry): Number of bays | - |
| L_mean | Input 3 (geometry): Mean bay length | m |
| L_max | Input 4 (geometry): Maximum bay length | m |
| L_min | Input 5 (geometry): Minimum bay length | m |
| W_total | Input 6 (geometry): Total frame width | m |
| h1 | Input 7 (geometry): First story height | m |
| htyp | Input 8 (geometry): Typical story height | m |
| Htot | Input 9 (geometry): Total height | m |
| cover | Input 10 (sections): Clear cover (outside stirrup) | m |
| col_B | Input 11 (sections): Column width | m |
| col_H | Input 12 (sections): Column depth | m |
| beam_b | Input 13 (sections): Beam width | m |
| beam_h | Input 14 (sections): Beam depth | m |
| rho_col | Input 15 (longitudinal): Column longitudinal ratio | - |
| rho_beam_top | Input 16 (longitudinal): Beam top steel ratio | - |
| rho_beam_bot | Input 17 (longitudinal): Beam bottom steel ratio | - |
| phi_long_col_mm | Input 18 (longitudinal): Column bar diameter | mm |
| phi_long_beam_mm | Input 19 (longitudinal): Beam bar diameter | mm |
| col_n_long | Input 20 (longitudinal): Column bar count | - |
| beam_n_long_top | Input 21 (longitudinal): Beam top bar count | - |
| beam_n_long_bot | Input 22 (longitudinal): Beam bottom bar count | - |
| col_phi_etr | Input 23 (transverse): Column stirrup diameter | m |
| col_s_etr | Input 24 (transverse): Column stirrup spacing (confined zone) | m |
| col_n_etr_x | Input 25 (transverse): Column tie legs, x | - |
| col_n_etr_y | Input 26 (transverse): Column tie legs, y | - |
| beam_phi_etr | Input 27 (transverse): Beam stirrup diameter | m |
| beam_s_etr | Input 28 (transverse): Beam stirrup spacing (confined zone) | m |
| rho_sh_col | Input 29 (confinement): Column volumetric transverse ratio | - |
| rho_sh_beam | Input 30 (confinement): Beam volumetric transverse ratio | - |
| rho_s_col | Input 31 (confinement): Column areal transverse ratio | - |
| rho_s_beam | Input 32 (confinement): Beam areal transverse ratio | - |
| omega_we_col | Input 33 (confinement): Effective mechanical confinement ratio, col (TBEC 2018 Eq. 5.4c) | - |
| omega_we_beam | Input 34 (confinement): Effective mechanical confinement ratio, beam (TBEC 2018 Eq. 5.4c) | - |
| alpha_se_col | Input 35 (confinement): Confinement effectiveness, col (TBEC 2018 Eq. 5.4d) | - |
| alpha_se_beam | Input 36 (confinement): Confinement effectiveness, beam (TBEC 2018 Eq. 5.4d) | - |
| fcc_col_MPa | Input 37 (confinement): Confined strength, col (Mander) | MPa |
| fcc_beam_MPa | Input 38 (confinement): Confined strength, beam (Mander) | MPa |
| fc_MPa | Input 39 (materials): Characteristic concrete strength | MPa |
| fy_MPa | Input 40 (materials): Steel yield strength (420/500) | MPa |
| G | Input 41 (loads): Dead line load | kN/m |
| Q | Input 42 (loads): Live line load | kN/m |
| soil_ord | Input 43 (site): Soil class as an ordinal code (ZA = 0 to ZE = 4) | - |
| col_is_tbdy_confined | Input 44 (flag): Column satisfies the confinement rules of TBEC 2018 | 0/1 |
| beam_is_tbdy_confined | Input 45 (flag): Beam satisfies the confinement rules of TBEC 2018 | 0/1 |
| soil_class | Soil class of TBEC 2018 (ZA to ZE); `soil_ord` is the same class as 0 to 4 | - |
| Vy_kN | Yield base shear of the idealized curve | kN |
| delta_y_m | Yield roof displacement of the idealized curve | m |
| Vmax_kN | Peak base shear | kN |
| delta_max_m | Roof displacement at the peak base shear | m |
| delta_u_m | Ultimate roof displacement (base shear at 80% of the peak on the descending branch, or end of the analysis) | m |
| T1_s | Elastic first-mode period | s |
| delta_t_m | Target roof displacement (single-mode procedure of TBEC 2018) | m |
| V_at_delta_t_kN | Base shear at the target displacement | kN |
| performance_level | Performance level at the target displacement: IO, LS, CP or C | - |
| curve_stored | 1 if the normalized curve of the frame is in `normalized_curves.csv` | - |
| curve_complete | 1 if the stored curve ends at 0.8 of the peak base shear (last ordinate within 0.02 of 0.8) | - |

## normalized_curves.csv

`v_00` to `v_20` are the base shears divided by the peak base shear at 21 equally spaced stations between zero and the ultimate displacement (`delta_u_m`). The curve in physical units is obtained with `delta_u_m` and `Vmax_kN` of `frames.csv`.

## predictions.csv

For the frames of the training pool the predictions are five-fold out-of-fold predictions; for the hold-out set they come from models fitted on the whole training pool.

| Column | Description |
|---|---|
| pred_Vy_kN ... pred_V_at_delta_t_kN | Predictions of the eight regressors, in the units of `frames.csv` |
| delta_t_computed_m | Target displacement computed from the predicted points with the code procedure (hold-out set) |
| delta_t_computed_actual_points_m | The same computation with the actual points (hold-out set) |
| p_multiclass_at_least_LS, _at_least_CP, _C | Probability of the multiclass model that the level is at least LS, at least CP, and C |
| p_ordinal_at_least_LS, _at_least_CP, _C | The same probabilities of the ordinal model |
| curve_error_point_based, _mean_shape, _station, _pca | Pointwise error of the four curves: median over the stations of the absolute difference in base shear, in percent of the actual peak base shear (hold-out frames with a stored curve) |

# DataModel (IMBH Catalog)

This document describes the structure for the **PRIMARY_DATA** table generated from the IMBH catalog and PyQSOFit outputs.

## Computation Rules (from wf_res)

- `logL_Ha_br` = log10(L_Ha) where L_Ha comes from `Ha_whole_br_area` flux using the same luminosity conversion as `calculate_bh_mass.py`.
- `FWHM_Ha_br` = `Ha_whole_br_fwhm`
- `logFWHM_Ha_br_err` = `FWHM_Ha_br_err / (FWHM_Ha_br * ln(10))`
- `logL_Hb_br` = log10(L_Hb) from `Hb_whole_br_area` (same conversion).
- `FWHM_Hb_br` = `Hb_whole_br_fwhm`
- `logFWHM_Hb_br_err` = `FWHM_Hb_br_err / (FWHM_Hb_br * ln(10))`
- `logM_BH` = 6.57 + 0.47*(logL_Ha_br - 42) + 2.06*(log10(FWHM_Ha_br) - 3)
- `logM_BH_err` = sqrt((0.47*logL_Ha_br_err)^2 + (2.06*logFWHM_Ha_br_err)^2)
- Narrow-line fluxes: per-component fits are in ln(λ).
  - Convert center and width: `lambda_center = exp(centerwave)` (Angstrom), `sigma_lambda = sigma * lambda_center`
  - `flux = scale * sigma_lambda * sqrt(2*pi)`
  - `flux_err` from propagation: `sqrt((sigma_lambda*sqrt(2*pi)*scale_err)^2 + (scale*sqrt(2*pi)*sigma_lambda_err)^2)`
  - If `scale > 15000`, set `flux = -1` and `flux_err = -1`

## PRIMARY_DATA Columns

| Column | Type | Source | Notes | Description |
|---|---|---|---|---|
| name | U50 | imbh_total_v2 | Name | Object name |
| alter_name | U50 | imbh_total_v2 | Alter_Name | Alternate name |
| RA | f8 | imbh_total_v2 | deg | Right ascension (J2000) in degrees |
| DEC | f8 | imbh_total_v2 | deg | Declination (J2000) in degrees |
| z | f8 | imbh_total_v2 | unitless | Redshift of the object |
| logM_star | f8 | imbh_total_sfr | log10(M_sun) | Stellar mass (log10) |
| logM_star_err | f8 | imbh_total_sfr | log10(M_sun) | Uncertainty on stellar mass |
| logM_star_source | i2 | imbh_total_sfr | unitless | Stellar mass source |
| logSFR | f8 | imbh_total_sfr | log10(M_sun/yr) | Star formation rate (log10) |
| logSFR_err | f8 | imbh_total_sfr | log10(M_sun/yr) | Uncertainty on star formation rate |
| logSFR_source | i2 | imbh_total_sfr | unitless | Star formation rate source |
| logL_Ha_arc | f8 | primary_data | erg/s (stored as log10 values) | Halpha archival line luminosity |
| FWHM_Ha_arc | i8 | primary_data | km/s (rounded to nearest integer) | Halpha archival FWHM |
| logM_BH_arc | f8 | primary_data | M_sun (stored as log10 values) | Archival black hole mass |
| ref | U50 | imbh_total_v2 | Ref | Reference |
| plate | i4 | imbh_total_v2 | plate | SDSS plate identifier |
| mjd | i4 | imbh_total_v2 | mjd | SDSS modified Julian date |
| fiber | i4 | imbh_total_v2 | fiber | SDSS fiber identifier |
| frac_host_5100 | f8 | wf_res | unitless | Host-galaxy fraction at 5100 A from spectral decomposition |
| SNR_host | f8 | wf_res | unitless | S/N for host continuum |
| SNR_Ha | f8 | wf_res | unitless | S/N for broad Ha |
| sigma_star | f8 | wf_res | km/s | Stellar velocity dispersion (kept only if SNR_host>3, sigma_star>sigma_star_err, and sigma_star<350; else -1) |
| sigma_star_err | f8 | wf_res | km/s | Uncertainty on stellar velocity dispersion (same reliability cut as sigma_star) |
| tag_ppxf | i2 | wf_res | Tag_ppxf == "Yes" (1/0) | Flag for pPXF-based host decomposition |
| tag_imbh | i8 | imbh_total_v2 | tag | Visual/selection tag |
| logL_5100 | f8 | wf_res | erg/s (stored as log10 values) | Total continuum luminosity at 5100 A (log10) |
| logL_5100_AGN | f8 | wf_res | erg/s (stored as log10 values) | AGN continuum luminosity at 5100 A (log10) |
| logL_Ha_br | f8 | wf_res | erg/s (stored as log10 values) | Broad Halpha luminosity (log10) |
| logL_Ha_br_err | f8 | wf_res | erg/s (stored as log10 values) | Uncertainty on broad Halpha luminosity (log10) |
| FWHM_Ha_br | f8 | wf_res | km/s | Broad Halpha FWHM |
| FWHM_Ha_br_err | f8 | wf_res | km/s | Uncertainty on broad Halpha FWHM |
| logL_Hb_br | f8 | wf_res | erg/s (stored as log10 values) | Broad Hbeta luminosity (log10) |
| logL_Hb_br_err | f8 | wf_res | erg/s (stored as log10 values) | Uncertainty on broad Hbeta luminosity (log10) |
| FWHM_Hb_br | f8 | wf_res | km/s | Broad Hbeta FWHM |
| FWHM_Hb_br_err | f8 | wf_res | km/s | Uncertainty on broad Hbeta FWHM |
| logM_BH | f8 | derived | M_sun (stored as log10 values) | Virial black hole mass estimated from broad Halpha |
| logM_BH_err | f8 | derived | M_sun (stored as log10 values) | Uncertainty on virial black hole mass |
| lam_Edd | f8 | derived | unitless | Eddington ratio using L5100_AGN * 9.26 and M_BH * 1.26e38 |
| FWHM_Ha_na | f8 | wf_res | km/s | Narrow Halpha FWHM |
| FWHM_OIIIc | f8 | wf_res | km/s | OIII5007 core FWHM |
| flux_OI6302 | f8 | wf_res_oxygen | erg/s/cm^2 | [O I] 6302 A line flux from qsopar_oxygen fit |
| flux_OI6302_err | f8 | wf_res_oxygen | erg/s/cm^2 | Uncertainty on [O I] 6302 A flux |
| flux_OII3728 | f8 | wf_res_oxygen | erg/s/cm^2 | [O II] 3728 A line flux from qsopar_oxygen fit |
| flux_OII3728_err | f8 | wf_res_oxygen | erg/s/cm^2 | Uncertainty on [O II] 3728 A flux |
| Voff_OIIIw | f8 | wf_res | km/s | OIII5007 wing velocity offset |
| Voff_OIIIc | f8 | wf_res | km/s | OIII5007 core velocity offset |
| Sig_OIIIw | f8 | wf_res | unitless | OIII5007 wing significance |
| Sig_OIIIc | f8 | wf_res | unitless | OIII5007 core significance |
| flux_Ha_narrow | f8 | wf_res | erg/s/cm^2 | Narrow Halpha line flux |
| flux_Ha_narrow_err | f8 | wf_res | erg/s/cm^2 | Uncertainty on narrow Halpha flux |
| flux_NII6549 | f8 | wf_res | erg/s/cm^2 | [N II] 6549 A line flux |
| flux_NII6549_err | f8 | wf_res | erg/s/cm^2 | Uncertainty on [N II] 6549 A flux |
| flux_NII6585 | f8 | wf_res | erg/s/cm^2 | [N II] 6585 A line flux |
| flux_NII6585_err | f8 | wf_res | erg/s/cm^2 | Uncertainty on [N II] 6585 A flux |
| flux_SII6718 | f8 | wf_res | erg/s/cm^2 | [S II] 6718 A line flux |
| flux_SII6718_err | f8 | wf_res | erg/s/cm^2 | Uncertainty on [S II] 6718 A flux |
| flux_SII6732 | f8 | wf_res | erg/s/cm^2 | [S II] 6732 A line flux |
| flux_SII6732_err | f8 | wf_res | erg/s/cm^2 | Uncertainty on [S II] 6732 A flux |
| flux_Hb_narrow | f8 | wf_res | erg/s/cm^2 | Narrow Hbeta line flux |
| flux_Hb_narrow_err | f8 | wf_res | erg/s/cm^2 | Uncertainty on narrow Hbeta flux |
| flux_OIII4959 | f8 | wf_res | erg/s/cm^2 | [O III] 4959 A total (core+wing) flux |
| flux_OIII4959_err | f8 | wf_res | erg/s/cm^2 | Uncertainty on [O III] 4959 A flux |
| flux_OIII5007 | f8 | wf_res | erg/s/cm^2 | [O III] 5007 A total (core+wing) flux |
| flux_OIII5007_err | f8 | wf_res | erg/s/cm^2 | Uncertainty on [O III] 5007 A flux |

## Missing Values

- Numeric columns: `-1`
- String columns: empty string

## Stellar Mass/SFR Sources

Sources (from IMBH_stellarmass.md):
1. DESI CIGALE: https://data.desi.lbl.gov/doc/releases/dr1/vac/cigale/
2. SDSS+WISE (Chang15): https://ui.adsabs.harvard.edu/abs/2015ApJS..219....8C/abstract
3. GSWLC DR2: https://salims.pages.iu.edu/gswlc/#catalog-download
4. NSA: https://www.sdss4.org/dr17/manga/manga-target-selection/nsa/

Priority codes used in logM_star_source / logSFR_source:
1 = DESI good
2 = GSWLC good
3 = Chang15 good
4 = DESI bad
5 = Chang15 bad
6 = NSA

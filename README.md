# CyberNeuro atlases

Reference atlases that CyberNeuro downloads the first time a
pipeline needs them, so the app and its skills do not ship them. The data live in the
release assets; the app pins every file's SHA-256 and verifies each download.

These atlases are the work of their authors. CyberNeuro only repackages them, unchanged,
under their original licences. **Please cite the original papers** (listed below and in
each pack's `SOURCE.md`).

## Infant atlases — release [`infant-atlases-v1`](https://github.com/MEKOMIKU/cyberneuro-atlases/releases/tag/infant-atlases-v1)

Used by the brain-mri skill's infant routes: NiBabies-based functional connectivity and
QSIPrep + ACT structural connectivity.

| Pack | Contents | Licence | Original source |
|---|---|---|---|
| `unc` | UNC 0-1-2 Infant Atlases: neonate (T2w), 1-year and 2-year (T1w) templates, tissue segmentations, AAL-90 parcellations, affines to MNI | CC BY 4.0 | <https://www.nitrc.org/projects/pediatricatlas> |
| `bcp` | UNC-BCP 4D Infant Brain Volumetric Atlas: monthly 00–12, then 15, 18, 21, 24, 36, 48 and 60 months; T1, T2, mask, tissue segmentation and Desikan-82 parcellation | GPL (as listed on NITRC; GPL-3.0 text included) | <https://www.nitrc.org/projects/uncbcp_4d_atlas/> |
| `dhcp_ga43`, `dhcp_ga44` | dHCP groupwise volumetric atlas, weeks 43 and 44 PMA: T2w template and 87 structures | CC BY 4.0 | <https://gin.g-node.org/BioMedIA/dhcp-volumetric-atlas-groupwise> |
| `ena50` | Edinburgh Neonatal Atlas ENA50: templates, diffusion maps, tissue maps and five parcellations (ENA-107, JHU-122, M-CRIB-84, AAL-90, dHCP-87) in one neonatal space | GPL-3.0 | <https://git.ecdf.ed.ac.uk/jbrl/ena> |
| `mcrib` | M-CRIB neonatal group template (100 regions) | not stated by the authors | **Not mirrored here.** Downloaded from the authors' repository, [DevelopmentalImagingMCRI/M-CRIB_atlas](https://github.com/DevelopmentalImagingMCRI/M-CRIB_atlas), at commit `ca588b4`. |

Each `<pack>.tar.gz` unpacks into a folder named after the pack. Unpack them all into one
folder and point the scripts at it with `ATLAS_BASE_DIR` (the brain-mri skill's
`assets/atlases/` layout):

```
atlases/
  unc/infant-{neo,1yr,2yr}{,-aal,-seg,-seg-{wm,gm,csf}}.nii.gz
  bcp/<MM>Month/BCP-<MM>M-{T1,T2,Mask,Seg,Parc}.nii.gz
  dhcp_ga43/{template_t2,structures}.nii.gz
  dhcp_ga44/{template_t2,structures}.nii.gz
  ena50/ENA50_*.nii.gz, *_lut.txt
  mcrib/{t1,t2}_M-CRIB_template.nii.gz, M-CRIB_labels.nii.gz
```

[`infant-atlases/infant-atlases.json`](infant-atlases/infant-atlases.json) lists every pack
with its download URLs, SHA-256 checksums and files. [`SHA256SUMS`](infant-atlases/SHA256SUMS)
holds the same checksums in `sha256sum -c` format; the M-CRIB lines are for the files
fetched from the authors' repository.

### Getting them

- **In CyberNeuro:** nothing to do. The fc-infant, sc-infant and brain-mri launchers
  download the packs a run needs into CyberNeuro's cache and reuse them afterwards.
- **brain-mri skill on a cluster:** `bash scripts/fetch_atlases.sh --download`. To fetch
  only some packs, name them, e.g. `--download unc dhcp_ga44`.
- **By hand:**

  ```bash
  base=https://github.com/MEKOMIKU/cyberneuro-atlases/releases/download/infant-atlases-v1
  curl -fLO "$base/unc.tar.gz" && curl -fLO "$base/SHA256SUMS"
  sha256sum -c --ignore-missing SHA256SUMS && tar -xzf unc.tar.gz -C /path/to/atlases
  ```

### What changed from the originals

The atlas files are byte-identical to the copies the lab's brain-mri skill bundles.
Packaging adds the following:

- a `SOURCE.md` in every pack;
- a copy of the licence text where the original download had none (`bcp`, `dhcp_ga43`,
  `dhcp_ga44`).

Some files sit under the folder names the scripts expect; for example, dHCP weeks 43 and
44 are in their own folders.

### Citations

- **UNC 0-1-2:** Shi F, Yap P-T, Wu G, Jia H, Gilmore JH, Lin W, Shen D. Infant brain
  atlases from neonates to 1- and 2-year-olds. *PLoS ONE* 6(4):e18746, 2011.
- **UNC-BCP 4D:** Chen L, et al., *NeuroImage* 2022 (the full reference is on the NITRC
  page).
- **dHCP:** Schuh A, et al. Unbiased construction of a temporally consistent
  morphological atlas of neonatal brain development. *bioRxiv* 251512, 2018.
- **ENA50:** Blesa M, et al. (ENA33, *Front Neurosci* 2016; ENA50). The source repository
  lists the papers to cite.
- **M-CRIB:** Alexander B, et al., *NeuroImage* 2017:
  <http://www.sciencedirect.com/science/article/pii/S1053811916305444>

Licence texts: [`LICENSES/CC-BY-4.0.txt`](LICENSES/CC-BY-4.0.txt) and
[`LICENSES/GPL-3.0.txt`](LICENSES/GPL-3.0.txt).

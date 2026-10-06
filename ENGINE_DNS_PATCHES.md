# Engine DNS solver patches

This branch is based on official NekRS v26.0 commit
`96b3cf9e5bacede16568826c04a21bc0fe50dc7d`. The same commit is the
`NekEngine/nekRS` fork's `master` at the time this branch was prepared.

The source changes are limited to two Nek5000 files:

- `core/ic.f` allocates the large crystal-routing restart buffers on reader
  ranks at run time. This avoids the AArch64 static-data relocation overflow
  observed in the 64-node engine build. The allocation size is unchanged.
- `core/gfldr.f` uses a smaller distributed search hash, searches target points
  in collective batches of 250,000 per rank, and evaluates each batch after
  resetting GSLIB's cached routing. These changes are required for the native
  `+int` restart between the large engine meshes.

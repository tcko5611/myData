```toc
  min_depth: 1
```
# Modules
- emacs 25.1
- python 3.6.3
- cmake 3.6.2
# P4
- /remote/port4/PEtools/CMT2/latest/p4commit
# qsh
- source /remote/sge1/default/ams_cae_grid/common/settings.csh
- qsh -P iheavy -V -l os_distribution=centos -l qsc=o -cwd -- -sl 6000 -sb -fn 9x15 -bg black -fg yellow
- source /remote/sge/cells/snps/common/settings.csh
- quser -P gpu
# qt
/depotbld/RHEL7.0/qt-creator-4.0.2/bin/qtcreator
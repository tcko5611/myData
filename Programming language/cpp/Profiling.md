           

# Valgrind command
Module load valgrind/3.13.0
valgrind --tool=callgrind --callgrind-out-file=tt.prof customfault -c config.xa.sqa.json -t sqa -o CF/sqa

# kcachegrind command
module load kcachegrind
kcachegrind tt.prof

![[kcahegrind.png]]
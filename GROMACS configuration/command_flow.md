# GROMACS commands list
1. gmx pdb2gmx -f protein.pdb -o conf.gro -water tip3p -ignh -missing -quiet
    * input: 1
2. gmx editconf -f conf.gro -o box.gro -c -d 1.0 -bt cubic -quiet
3. gmx solvate -cp box.gro -cs spc216.gro -o box_s.gro -p topol.top -quiet
4. gmx grompp -f /root/gmxrunner/data/ions.mdp -c box_s.gro -p topol.top -o ions.tpr -maxwarn 2 -quiet
5. gmx genion -s ions.tpr -o box_s_i.gro -p topol.top -pname SOD -nname CLA -neutral -quiet
    * input: 13
6. gmx grompp -f /root/gmxrunner/data/em.mdp -c box_s_i.gro -p topol.top -o em.tpr -maxwarn 2 -quiet
7. gmx mdrun -deffnm em -nb cpu -pme cpu -nt 4 -quiet
8. gmx make_ndx -f em.gro -quiet
    * input: "q"
9. gmx grompp -f /root/gmxrunner/data/heat.mdp -c em.gro -r em.gro -p topol.top -o heat.tpr -maxwarn 2 -quiet
10. gmx mdrun -deffnm heat -gpu_id 0 -nt 50 -quiet
11. gmx grompp -f /root/gmxrunner/data/md.mdp -c heat.gro -t heat.cpt -p topol.top -o md.tpr -maxwarn 2 -quiet
12. gmx mdrun -deffnm md -gpu_id 0 -nt 50 -quiet

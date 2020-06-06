% Extracts voxelwise values from PET images in each AAL atlas region.
% (https://www.meduniwien.ac.at/neuroimaging/downloads.html)

% AAL atlas downloaded from: https://www.gin.cnrs.fr/en/tools/aal/
% - AAL3 used here, but any version will work.

% Raw downloaded .hdr files are registered to a non-standard MNI space
% (that is 79x95x69 voxels). Since the AAL atlas is in 2mm 91x109x91 MNI
% space, the raw files must be linearly registered to this space as well.
% (Used FLIRT with reference MNI152_T1_2mm_brain.nii.gz from FSL)
% June 5 2020 Emily Olafson emo4002@med.cornell.edu

% Make sure spm12 is added to the MATLAB path and the AAL atlas is
% downloaded.
clear all; 

% Unzip outputs of FLIRT (since SPM functions don't like compress;ed files)
gunzip('/Users/emilyolafson/Downloads/WAY/outputflirt.nii.gz');

% Extracts voxelwise PET values.
file = spm_vol('/Users/emilyolafson/Downloads/WAY/outputflirt.nii');
PETvoxels = spm_read_vols(file); %matrix of <91><109><91> voxels
PETvoxels_reshape=reshape(PETvoxels, [902629 1]);

% Load AAL atlas.
atlas = spm_vol('/Users/emilyolafson/Desktop/spm12/toolbox/AAL3/AAL3.nii');
atlasvoxels = spm_read_vols(atlas);
atlasvoxels_reshape=reshape(atlasvoxels, [1 902629]);

% Load atlas lookup table.
% AAL3.nii has only non-zero integer values for each region, not names.
% This file contains the mapping between integer valus & region names.
filename = '/Users/emilyolafson/Desktop/spm12/toolbox/AAL3/AAL3.nii.txt';
delimiter = ' ';
headerlinesIn = 0;
atable = importdata(filename, delimiter, headerlinesIn);

% Make a separate table for each region & save to folder.
for i=1:size(atable.data,1)
    a.nvoxels = sum(atlasvoxels(:)==i); %calculate total #voxels in the region.
    PETvalues = PETvoxels(find(atlasvoxels==i)); %find PET values in voxels in region.
    idx = find(atlasvoxels==i); %find linear index of voxels in region.
    [vx,vy,vz] = ind2sub([91 109 91], idx); %find exact voxel coordinates
    a.name = atable.textdata(i,2);
    a.number = i;
    a.coords = [PETvalues,vx, vy, vz]
    structname = char(a.name)
    writematrix(a.coords, structname)
   % save(structname, '-struct','a') %option to save each file as its own
   % struct with more information.
end
    

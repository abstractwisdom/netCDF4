#!/bin/env python3
import matplotlib.pyplot as plt
import numpy as np
from netCDF4 import Dataset
import argparse
from mpl_toolkits.basemap import Basemap


parser = argparse.ArgumentParser()

parser.add_argument('-d', '--dataset', required=True, help='NETCDF Datafile')
parser.add_argument('-r', '--datarange', required=True, help='NETCDF Datafile')

args = parser.parse_args()

netdata = args.dataset
datarange = args.datarange
datarange = int(datarange)

def create_cdf(jposo, iposo, len_nps, len_time, time_o):
    dataset = Dataset('merged.nc', 'w')
    print(len_nps)
    nps_c = dataset.createDimension('nps', None)
    time_c = dataset.createDimension('time', len_time)
    time_d = dataset.createVariable('time', np.float64, ('time'))
    ipos_c = dataset.createVariable('ipos', np.float64, ('time','nps'))
    jpos_c = dataset.createVariable('jpos', np.float64, ('time', 'nps'))

    time_d[:] = time_o
    jpos_c[:] = jposo
    ipos_c[:] = iposo

def append_cdf(jposo, iposo, num_parts, len_time, time_o, num_parts1):
    dataset = Dataset('merged.nc', 'a')
    print(num_parts)
    print(num_parts1)
    nps_c = dataset.dimensions['nps']
    ipos_c = dataset.variables['ipos'][:]
    jpos_c = dataset.variables['jpos'][:]

    print("Shape1", jposo.shape)
    jpos_c = jposo[:,num_parts1:num_parts]
    print("Shape2", jpos_c.shape)
    ipos_c = iposo[:,num_parts]
    
created_file = 0
num_parts = 0
num_parts1 = 0


for i in np.arange(1,datarange):
    netdata = args.dataset
    try:
        if netdata is not None:
            particle_data = Dataset(netdata + str(i) + '.nc','r')
            print(particle_data)

            nps_o = particle_data.dimensions['nps']
            time_o = particle_data.variables['time'][:]
            time_len = len(time_o)
            nps_len = len(nps_o)

            jpos_o = particle_data.variables['jpos'][:]
            ipos_o = particle_data.variables['ipos'][:]

            if created_file == 0:
                create_cdf(jpos_o, ipos_o, nps_len, time_len, time_o)
                created_file = created_file + 1
                num_parts1 = len(nps_o)
                num_parts = len(nps_o)
            else:
                print("New")
                num_parts += len(nps_o)
                append_cdf(jpos_o, ipos_o, num_parts, time_len, time_o, num_parts1)
    except:
        pass

    
    
    

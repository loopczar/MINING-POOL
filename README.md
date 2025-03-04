# [MINING-POOL](https://t.me/czarbit)
This code allows you to set automations for your mining pool

 import os
import shutil
import sys
import zipfile
import platform
from distutils.core import setup
from distutils.sysconfig import get_python_lib
import py2exe

version = import('p2pool').version
is_64bit = '64' in platform.architecture()[0]

extra_includes = []
extra_includes.extend(f'p2pool.networks.{x}' for x in import('p2pool.networks').nets)
extra_includes.extend(f'p2pool.bitcoin.networks.{x}' for x in import('p2pool.bitcoin.networks').nets)

init_file = os.path.join('p2pool', 'init.py')
init_backup = 'INITBAK'

if os.path.exists(init_backup):
    os.remove(init_backup)

os.rename(init_file, init_backup)
try:
    with open(init_file, 'w') as f:
        f.write(f"version = '{version}'\nDEBUG = False\n")
    
    mfc_dir = os.path.join(get_python_lib(), 'pythonwin')
    mfc_files = [os.path.join(mfc_dir, dll) for dll in [
        "mfc90.dll", "mfc90u.dll", "mfcm90.dll", "mfcm90u.dll", "Microsoft.VC90.MFC.manifest"
    ]]
    
    bundle_level = 3 if is_64bit else 1
    sys.argv[1:] = ['py2exe']

    IF YOU HAVE ANY QUESTION OR ENQUIRY< REACH OUT TO  # [ADMIN](https://t.me/czarbit)

    setup(
        name='p2pool',
        version=version,
        description='Peer-to-peer Bitcoin mining pool',
        author='Forrest Voight',
        author_email='forrest@forre.st',
        url='http://p2pool.forre.st/',
        data_files=[
            ('', ['README.md']),
            ("Microsoft.VC90.MFC", mfc_files),
            ('web-static', [
                'web-static/d3.v2.min.js',
                'web-static/favicon.ico',
                'web-static/graphs.html',
                'web-static/index.html',
                'web-static/share.html',
            ]),
        ],
        console=['run_p2pool.py'],
        options={'py2exe': {
            'bundle_files': bundle_level,
            'dll_excludes': ['w9xpopen.exe', "mswsock.dll", "MSWSOCK.dll"],
            'includes': ['twisted.web.resource', 'ltc_scrypt'] + extra_includes,
        }},
        zipfile=None,
    )
finally:
    os.remove(init_file)
    os.rename(init_backup, init_file)

arch = '64' if is_64bit else '32'
dist_dir = f'p2pool_win{arch}_{version}'

if os.path.exists(dist_dir):
    shutil.rmtree(dist_dir)

os.rename('dist', dist_dir)

with zipfile.ZipFile(f'{dist_dir}.zip', 'w', zipfile.ZIP_DEFLATED) as zipf:
    for root, _, files in os.walk(dist_dir):
        for file in files:
            zipf.write(os.path.join(root, file))

print(dist_dir)

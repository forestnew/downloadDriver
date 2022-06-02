import os
import shlex
from subprocess import Popen, PIPE

driver64_link = 'https://github.com/mozilla/geckodriver/releases/download/v0.31.0/geckodriver-v0.31.0-linux64.tar.gz'
driver32_link = 'https://github.com/mozilla/geckodriver/releases/download/v0.31.0/geckodriver-v0.31.0-linux32.tar.gz'

cm_del_old_file = 'rm -R webdriver'
cm_firefox_vers = 'firefox --version'

def run_command(command_line):
    if '&&' in command_line:
        for command in command_line.split('&& '):
            p = Popen(shlex.split(command), stdout=PIPE, stderr=PIPE)
            stdout, stderr = p.communicate()
    else:
        p = Popen(shlex.split(command_line), stdout=PIPE, stderr=PIPE)
        stdout, stderr = p.communicate()
    return stdout.decode("UTF-8"), stderr.decode("UTF-8")
def delete_old_driver():
    run_command(cm_del_old_file)
def check_firefox():
    sterr = run_command(cm_firefox_vers)[1]
    return sterr

def download_driver(os_info):
    if os_info.sysname == 'Linux':
        if '86' in os_info.machine:
            sterr = run_command(f'wget {driver64_link}')[1]
            file_name = driver64_link.split('/')[-1]
            assert os.path.isfile(file_name), 'File not found'
            return file_name

def unpack_arch(arch_name):
    sterr = run_command('mkdir -p webdriver && tar -C webdriver -xvf %s && rm %s' %(arch_name,arch_name))[1]
    return True if sterr == '' else False, sterr


if __name__ == '__main__':
    assert check_firefox() == '', 'Firefox is not install'
    delete_old_driver()
    file_name = download_driver(os.uname())
    unpack = unpack_arch(file_name)
    if unpack[0] is True:
        print('Successfully completed')
    else:
        print('Ended with an error:\n', unpack[1])

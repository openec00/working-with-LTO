## Working with LTO8
Install tmux: https://tmuxcheatsheet.com/
```bash
sudo yum install tmux
```
<br />

Create a named tmux session 
```bash
sudo tmux new -s SESSION_NAME
```
<br />

Attach to tmux session
```bash
tmux attach –t SESSION_NAME
```
<br />

Detach from a session
```
CTRL-b then tap ‘d’ 
```
<br />

Listing source files
```bash
ls -lht /Path/To/Files
```
<br />

Check LTO8 Device status (Autoloader)
```bash
mtx -f /dev/sg4 status
```
<br />

Example Output:
###### Storage Element 10:Full :VolumeTag=QLZ040L8
###### Storage Element 11:Empty
<br />

Load tape - Loading slot 4 into drive 0
```bash
mtx -f /dev/sg4 load 10 0
```
<br />

Format the LTO tape
```bash
mkltfs --device=/dev/st0 --backend=ltotape --volume-name="QLZ040L8"
```
<br />

Mount the LTO tape
```bash
ltfs /mnt/ltfs_st0 -o tape_backend=ltotape,sync_type=unmount,uid=1000,gid=100,umask=022,devname=/dev/nst0
```
<br />

Write files to LTO using mbuffer to handle I/O and create logfile and md5 checksum
```bash
mbuffer -i 2014_Hanzo.tar -l /var/log/MYFILES_QLZ040L8.log -e --hash MD5 -m 2G -P 10 -c -o /mnt/ltfs_st0/MYFILES.tar
```
<br />

Unmount ltfs filesystem
```bash
umount /mnt/ltfs_st0
```
<br />

Verify that the ltfs filessystem has finished unmounting - BOT shopuld be returned in the output before you continue
```bash
mtx -f /dev/sg4 unload 10 0
```
<br />

```bash
Detach from tmux session
```CTRL-b then tap ‘d’
```
<br />

Kill tmux session
```bash
tmux kill-session -t SESSION_NAME
```
<br />

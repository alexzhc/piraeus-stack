
## Overview 
Piraeus project builds cloud-native features that are tightly coupled with Kubernetes mechanisms, therefore its stack consists of its own api, cli, csi and gui.

## Important features:

### ReadWriteMany
Export block volume by host NFS server and use kubernetes cluster ip as the NFS VIP. A workflow is demonstrated by following child project
> volume nfs provisioner: https://github.com/alexzhc/volume-nfs-provisioner

### Fencing
Instead of waiting for kubernetes node eviction timeout (5 minutes) and CSI detach timeout (6 minutes), piraeus calls kubernetes api to forcefully delete and reschedule pod right after DRBD has failed over the resource. It improves TTR (time to recover) from 11 minutes down to 30-60 minutes.
>Ref: https://docs.storageos.com/docs/concepts/fencing

### Remote Backup
A kubernetes job object that mounts a volume snapshort by file or block driver, then does backup to remote S3 or NFS target by rsync or lvmsync.
>Ref: https://github.com/davidbartonau/lvm-thin-sendrcv
<br/>Ref: https://github.com/tasket/wyng-backup

### Scheduler extension
Replace volume node affinity with a scheduler extention to schedule the pod on the node with local volume access (diskfull), and if not possible schedule the pod on the node with remote volume access (diskless), similar to basic function of Stork. 
>Ref: https://kubernetes.io/docs/concepts/scheduling/scheduling-framework/

## Technical Stack

<table class="relative-table wrapped confluenceTable active-resizable" style="letter-spacing: 0px;"><colgroup><col style="width: 90px;" data-resize-pixel="90" data-resize-percent="11.74934725848564" data-offset-left="40" data-offset-right="130" /><col style="width: 169px;" data-resize-pixel="169" data-resize-percent="22.06266318537859" data-offset-left="130" data-offset-right="299" /><col style="width: 131px;" data-resize-pixel="131" data-resize-percent="17.10182767624021" data-offset-left="299" data-offset-right="430" /><col style="width: 47px;" data-resize-pixel="47" data-resize-percent="6.135770234986945" data-offset-left="430" data-offset-right="477" /><col style="width: 131px;" data-resize-pixel="131" data-resize-percent="17.10182767624021" data-offset-left="477" data-offset-right="608" /><col style="width: 106px;" data-resize-pixel="106" data-resize-percent="13.838120104438643" data-offset-left="608" data-offset-right="714" /><col style="width: 92px;" data-resize-pixel="92" data-resize-percent="12.010443864229766" data-offset-left="714" data-offset-right="806" /></colgroup><tbody><tr><th class="confluenceTh" colspan="3">&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; Control Plane</th><th class="confluenceTh" rowspan="10"><br /></th><th class="confluenceTh" style="text-align: center;" colspan="3">Data Plane</th></tr><tr><td class="confluenceTd" rowspan="5"><p><br /></p><p><br /></p><p><br /></p><p><br /></p><p><br /></p><p style="text-align: center;"><span style="color: #0000ff;">Piraeus</span></p><p><br /></p></td><td class="confluenceTd" style="text-align: center;" colspan="1"><p><span style="color: #0000ff;">User Interaction</span></p><p><span style="color: #0000ff;">Monitoring</span></p><p><span style="color: #0000ff;">Analysis</span></p></td><td class="confluenceTd" style="text-align: center;" colspan="1"><p><br /></p><p><span style="color: #0000ff;">GUI</span></p></td><td class="confluenceTd" style="text-align: center;" colspan="1"><p><br /></p><p><span style="color: #0000ff;">Async</span></p></td><td class="confluenceTd" style="text-align: center;" colspan="1"><br /></td><td class="confluenceTd" rowspan="5"><p><br /></p><p><br /></p><p><br /></p><p><br /></p><p><br /></p><p style="text-align: center;"><span style="color: #0000ff;">Piraeus</span></p></td></tr><tr><td class="confluenceTd" style="text-align: center;" colspan="1"><p><span style="color: #0000ff;">Advanced Scheduling</span></p><p><span style="color: #0000ff;">Storage-Influenced Fencing</span></p></td><td class="confluenceTd" style="text-align: center;" colspan="1"><span style="color: #0000ff;">Scheduler Extension</span></td><td class="confluenceTd" style="text-align: center;" colspan="1"><p><span style="color: #0000ff;">Remote Backup</span></p></td><td class="confluenceTd" style="text-align: center;" colspan="1"><br /></td></tr><tr><td class="confluenceTd" style="text-align: center;" colspan="1"><p><span style="color: #0000ff;">Resource Management</span></p></td><td class="confluenceTd" style="text-align: center;" colspan="1"><span style="color: #0000ff;">CLI, API</span></td><td class="confluenceTd" style="text-align: center;" colspan="1"><span style="color: #0000ff;">Remote Clone</span></td><td class="confluenceTd" style="text-align: center;" colspan="1"><br /></td></tr><tr><td class="confluenceTd" style="text-align: center;"><p><span style="color: #0000ff;">Deployment</span></p><p><span style="color: #0000ff;">Auto-Scaling</span></p></td><td class="confluenceTd" style="text-align: center;"><p><span style="color: #0000ff;">Operator</span></p></td><td class="confluenceTd" style="text-align: center;" colspan="1"><p><span style="color: #0000ff;">S3 Object</span></p></td><td class="confluenceTd" style="text-align: center;" colspan="1"><p><span style="color: #0000ff;">Minio</span></p></td></tr><tr><td class="confluenceTd" style="text-align: center;"><p><span style="color: #0000ff;">Storage Driver,</span></p><p><span style="color: #0000ff;">Dynamic Provisioning</span></p><p><span style="color: #0000ff;">Per-Volume Policy</span></p></td><td class="confluenceTd" style="text-align: center;"><p><br /></p><p><span style="color: #0000ff;">CSI</span></p></td><td class="confluenceTd" style="text-align: center;" colspan="1"><p><br /></p><p><span style="color: #0000ff;">ReadWriteMany</span></p></td><td class="confluenceTd" style="text-align: center;" colspan="1"><p><br /></p><p><span style="color: #0000ff;">NFS</span></p></td></tr><tr><td class="confluenceTd" colspan="3"><br /></td><td class="confluenceTd" colspan="3"><br /></td></tr><tr><td class="confluenceTd" rowspan="3"><br /><br /><br /></td><td class="confluenceTd" style="text-align: center;">DRBD Configuration</td><td class="confluenceTd" style="text-align: center;">LINSTOR</td><td class="confluenceTd" style="text-align: center;" colspan="1"><br /></td><td class="confluenceTd" style="text-align: center;" colspan="1"><br /></td><td class="confluenceTd" rowspan="3"><br /><br /><br /></td></tr><tr><td class="confluenceTd" style="text-align: center;"><br /></td><td class="confluenceTd" style="text-align: center;"><br /></td><td class="confluenceTd" style="text-align: center;" colspan="1">Data Replication</td><td class="confluenceTd" style="text-align: center;" colspan="1">DRBD</td></tr><tr><td class="confluenceTd" style="text-align: center;"><br /></td><td class="confluenceTd" style="text-align: center;"><br /></td><td class="confluenceTd" style="text-align: center;" colspan="1"><p>Volume Manager</p><p>COW</p></td><td class="confluenceTd" style="text-align: center;" colspan="1">LVM, Zvol</td></tr></tbody></table>


## Control Flow
```
+-------------+      +-------------+      +-------------+
| Piraeus GUI |      | Piraeus CLI |      | Piraeus CSI +--------------------+
+------+------+      +------+------+      +------+------+                    |
       |                    |                    |                           |
       |                    |                    |                           v
       |                    v                    |               +-----------+------------+
       +------------>+------+------+<------------+               |   Temporary  Plugin    |
                     | Piraeus API |                             +------------------------+
                  +--+-------------+--+                          | volume nfs provisioner |
                  |                   |                          | (nested storageclass)  |
                  |                   |                          +-----------+------------+
                  |                   |                                      |
                  v                   v                                      |
           +------+------+     +------+------+     +-------------+           |
           |   K8S API   |     | Linstor API +<----+ Linstor CSI +<----------+
           +------+------+     +------+------+     +-------------+
                  |                   |
                  v                   v
           +------+------+     +------+------+
           |  Advanced   |     |   Linstor   |
           |  Features   |     +------+------+
           +-------------+            |
                                      v
                               +------+------+
                               |DRBD/LVM/ZVOL|
                               +-------------+
```

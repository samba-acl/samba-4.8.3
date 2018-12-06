# samba-4.8.3
This Repository holds NFS4ACL_XATTR Plugin changes on Samba 4.8.3 which can be summarized as:

* Implemented another set of XDR structure and APIs which are compliant with NFS4 ACL Format prescribed in RFC 7530
* Changed XDR attribute name to "system.nfs4_acl" which works with native NFSv4 and nfs-ganesha.
* All owners with Special IDs along with OWNER@, GROUP@, EVERYONE@ are encoded/decoded now.
* Did not change any of the existing XDR Backend APIs (with nfs4acl prefix) which probably intend to support both NFS 4.0 and NFS 4.1 ACL format in future using nfs_version config option. Current APIs use iflag in the nfsace4 structure which I did not find even in NFS v4.1 Spec RFC 5661. And that was causing issues in encoding/decoding XDR blobs. So the existing code is broken for my tests and needed lot of fixes if it has to support both specs.


The patch file can be used in following steps:

1. Download Samba 4.8.3 with:
wget https://download.samba.org/pub/samba/stable/samba-4.8.3.tar.gz  

2. untar with: 
gunzip samba-4.8.3.tar.gz 
tar -xf samba-4.8.3.tar

3. Apply the patch with: 
cd samba-4.8.3
patch -p3 < nfs4acl_xattr.patch

4. Configure and Build samba with: 
./configure --disable-cups --without-quotas --without-ad-dc --without-pam --enable-fhs --prefix=/usr --sysconfdir=/etc/ --localstatedir=/var --with-modulesdir=/usr/lib64/samba/ configure

followed by 

make -j4

5. The plugin will be available at bin/default/modules/vfs/nfs4acl_xattr.so 

6. While creating the share use following options
        vfs objects = nfs4acl_xattr
        nfs4acl_xattr:xattr_name = system.nfs4_acl
        nfs4acl_xattr:version = 40
        nfs4acl_xattr:encoding = xdr40
        


* Alternatively, there is a Samba-4.8.3.tar.bz2 file where patch is already applied and can be configured and built using Step 4.
* nfs4acl_xattr.so compiled with Samba-4.8.3 is also uploaded, which can be placed in modules directory of your running samba and can be used with options specified in Step 6. 








# samba-4.8.3
This Repository holds NFS4ACL_XATTR Plugin changes on Samba 4.8.3 which can be summarized as:

* Implemented another set of XDR structure and APIs which are compliant with NFSv4 ACL Format prescribed in RFC 7530
* Changed XDR attribute name to "system.nfs4_acl" which works with native NFSv4 and nfs-ganesha.
* All owners with Special IDs along with OWNER@, GROUP@, EVERYONE@ are encoded/decoded now.


The patch file can be used in following steps:

1. Download Samba 4.8.3 with:

		wget https://download.samba.org/pub/samba/stable/samba-4.8.3.tar.gz  

2. untar with: 

		gunzip samba-4.8.3.tar.gz 

		tar -xf samba-4.8.3.tar

3. Apply the patch with: 
	
		cd samba-4.8.3
		patch -p4 < ../nfs4acl_xattr.patch

4. Configure and Build samba with: 

		./configure --disable-cups --without-quotas --without-ad-dc --without-pam --enable-fhs --prefix=/usr --sysconfdir=/etc/ --localstatedir=/var --with-modulesdir=/usr/lib64/samba/ configure

followed by 

	make -j4

Please note, the configuration options provided are only example. 

5. The plugin will be available at bin/default/modules/vfs/nfs4acl_xattr.so 

6. Place the nfs4acl_xattr.so in modules/vfs/ directory of your Samba. You can obtain modules/ directory using
			
			<path to your smbd> -b | grep MODULESDIR
			

6. While creating the share, use following options

        vfs objects = nfs4acl_xattr
	
        nfs4acl_xattr:xattr_name = system.nfs4_acl
	
        nfs4acl_xattr:version = 40
	
        nfs4acl_xattr:encoding = xdr40
        


* Alternatively, there is a "Samba-4.8.3.tar.bz2" file where patch is already applied and can be configured and built using Step 4.
* nfs4acl_xattr.so compiled with Samba-4.8.3 is also uploaded, which can be placed in modules/vfs/ directory of your running samba and can be used with options specified in Step 6. Obtain modules/ directory with following command:

		<path to your smbd> -b | grep MODULESDIR

Reload the configuration if you happen to change any of the share parameters

		smbcontrol all reload-config










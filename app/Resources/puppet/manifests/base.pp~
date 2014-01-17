Exec { path => [ "/bin/", "/sbin/" , "/usr/bin/", "/usr/sbin/" ] }

exec { "apt-update":
	command => "/usr/bin/apt-get update",
}

package { ['python-software-properties']:
	  ensure  => 'installed',
	  require => Exec['apt-update'],
}

exec { 'add-php5':
	command => "add-apt-repository ppa:ondrej/php5 && /usr/bin/apt-get update",
	require => Package['python-software-properties'],
}

$sysPackages = [ 'build-essential', 'curl']
package { $sysPackages:
  	ensure => "installed",
  	require => Exec['apt-update'],
}

package { 'php5':
	require => Exec['add-php5'],
}

$dependencies = [
	"php5-cli",
	"php5-curl",
	"php5-mysql",
	"php5-intl",
	"php5-mcrypt",
	"php5-imagick",
]
package { $dependencies:
	ensure  => present,
	require => Exec['apt-update'],
}

file { "/var/www/app.local":
	ensure  => "directory",
	owner   => "www-data",
	group   => "www-data",
	mode    => "0775",
	recurse => true,
}

class { "apache": 
	mpm_module => "prefork",
}

class { "apache::mod::php": }

apache::vhost { "app.local":
	priority   => 000,
	port       => 80,
	docroot    => "/var/www/app.local/web",
	ssl        => false,
	servername => "app.local",
	options    => ["FollowSymlinks MultiViews"],
	override   => ["All"],
	ensure     => present,
	require    => File['/var/www/app.local'],
}

user { 'symfony':
    	ensure     => present,
}

class { "::mysql::server":
    	root_password => "root"
}
mysql::db { 'mydb':
	user     => 'symfony',
	password => 'symfony',
	host     => 'localhost',
	grant    => ['ALL'],
}

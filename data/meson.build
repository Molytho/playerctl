application_id = 'org.mpris.MediaPlayer2.playerctld'

service_conf = configuration_data()
service_conf.set('application_id', application_id)
service_conf.set('bindir', bindir)
service_conf.set('prefix', prefix)

dbus_service = application_id + '.service'
systemd_service = 'playerctld.service'
systemd_service_install_dir = join_paths(libdir, 'systemd', 'user')

configure_file(
  input: dbus_service + '.in',
  output: dbus_service,
  install: true,
  install_dir: join_paths(datadir, 'dbus-1', 'services'),
  configuration: service_conf
)

configure_file(
	input: systemd_service + '.in',
	output: systemd_service,
	install: true,
	install_dir: systemd_service_install_dir,
	configuration: service_conf
)

install_symlink(
	'dbus-' + application_id + '.service',
	pointing_to: systemd_service,
	install_dir: systemd_service_install_dir
)

if get_option('bash-completions')
	bash_files = files(
		'playerctl.bash',
	)
	if bash_comp.found()
		bash_install_dir = bash_comp.get_pkgconfig_variable('completionsdir')
	else
		bash_install_dir = join_paths(datadir, 'bash-completion', 'completions')
	endif

	install_data(bash_files, install_dir: bash_install_dir)
endif

if get_option('zsh-completions')
	zsh_install_dir = join_paths(datadir, 'zsh', 'site-functions')
	install_data('playerctl.zsh', install_dir: zsh_install_dir, rename: '_playerctl')
endif

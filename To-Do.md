# CycurID BFA integration into Webmin

This document needs to be updated with the purpose to integrate CycurID BFA in to the webmin list of providers for 2FA.

## TODO

- [x] Add CycurID to the list of providers
- [ ] Create all the language files


### Create language files in twofactor.lang.auto.html/twofactor.lang.html

Need to add for content to the language files base don the below template:

```
<dt><b>Google Authenticator</b>
<dd>This is a smartphone app that implements the standard TOTP protocol. Each
    user must scan a QR code using the app to link their tokens with the
    Webmin server. <p>
```
### Notes

When TOTP is enabled, the user needs to enter the code during the login process. 

#### Important files
save_twofactor.cgi


```
This code is used generate the enrollement once the 2FA method is selected:
twofactor_form.cgi - line 50-63
my ($prov) = grep { $_->[0] eq $miniserv{'twofactor_provider'} }
		       &webmin::list_twofactor_providers();
	print &text($in{'user'} ? 'twofactor_desc2' : 'twofactor_desc',
		    "<i>$prov->[1]</i>",
		    $prov->[2],
		    "<tt>$in{'user'}</tt>"),"<p>\n";
	my $ffunc = "webmin::show_twofactor_form_".
		    $miniserv{'twofactor_provider'};
	if (defined(&$ffunc)) {
		print &ui_table_start($text{'twofactor_header'}, undef, 2);
		print &{\&{$ffunc}}($user);
		print &ui_table_end();
		}
	@buts = ( [ "enable", $text{'twofactor_enable'} ] );
```


```
This code is used to enroll into the Google Authenticator Once the 2FA method is selected:
twofactor-funcs-lib.pl  - line 173
# show_twofactor_form_totp(&user)
# Show form allowing the user to choose a twofactor secret
sub show_twofactor_form_totp
{
my ($user) = @_;
my $secret = $user->{'twofactor_id'};
$secret = undef if ($secret !~ /^[A-Z0-9=]+$/i ||
		    (length($secret) != 16 && length($secret) != 26 && length($secret) != 32));
my $rv;
$rv .= &ui_table_row($text{'twofactor_secret'},
	&ui_opt_textbox("totp_secret", $secret, 20, $text{'twofactor_secret1'},
			$text{'twofactor_secret0'}));
return $rv;
}
```
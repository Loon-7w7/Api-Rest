/*
-- Autenticacion via HTTP
$user = array_key_exists('PHP_AUTH_USER', $_SERVER ) ?  $_SERVER['PHP_AUTH_USER'] : '';
$pwd= array_key_exists('PHP_AUTH_PW', $_SERVER ) ?  $_SERVER['PHP_AUTH_PW'] : '';
if ($user !== 'Loon' || $pwd !== '1234') {
    echo 'Inicio de session: NO aprobado' . PHP_EOL;
    die;
} else {
    echo 'Inicio de session: Aprobado' . PHP_EOL;
}
*/
/*
// Autenticacion via HMAC
if (!array_key_exists('HTTP_X_HASH',$_SERVER) || !array_key_exists('HTTP_X_TIMESTAMP',$_SERVER) || !array_key_exists('HTTP_X_UID',$_SERVER)) {
	die;
}

list($hash,$uid,$timestamp) = [
	$_SERVER['HTTP_X_HASH'],
	$_SERVER['HTTP_X_UID'],
	$_SERVER['HTTP_X_TIMESTAMP']
];

$secret = 'Sh!! No se lo cuentes a nadie!';

$newHash = sha1($uid.$timestamp.$secret);

if ($newHash !== $hash) {
	die;
}
*/

/Autenticación via Access TOKEN
header( 'Content-Type: application/json' );

if ( !array_key_exists( 'HTTP_X_TOKEN', $_SERVER ) ) {

    die;
}

$url = 'https://localhost:8001';

$ch = curl_init( $url );
curl_setopt( $ch, CURLOPT_HTTPHEADER, [
    "X-Token: {$_SERVER['HTTP_X_TOKEN']}",
]);
curl_setopt( $ch, CURLOPT_RETURNTRANSFER, true );
$ret = curl_exec( $ch );


if ( $ret !== 'true' ) {
    http_response_code( 403 );
    die;
}
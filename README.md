MIT License

Copyright (c) 2022 OlympiCloud by WECREATIVE DIGITAL

```c#
app.MapPost("/login", [AllowAnonymous] async
    ([FromBodyAttribute] Users userModel, TokenService tokenService, IUserRepositoryService userRepositoryService, HttpResponse response, DbContextMaster db) =>
{

    dynamic userDto = null;

    if (userModel.dni != null)
    {
        userDto = userRepositoryService.GetUser(userModel, db);
    }

    if (userDto == null)
    {
        response.StatusCode = 401;
        return;
    }
    var token = tokenService.BuildToken(builder.Configuration["Jwt:ServerSecret"], builder.Configuration["Jwt:Issuer"], builder.Configuration["Jwt:Audience"], userDto);
    string role = userDto.FK_role.ToString();
    string verification = userDto.verification.ToString();
    string birthday = userDto.birthday.ToString();
    string createAt = userDto.created_at.ToString();
    string updateAt = userDto.update_at.ToString();
    string is_active = userDto.is_active.ToString();
    string id = userDto.Id.ToString();
    await response.WriteAsJsonAsync(new
    {
        Id = id,
        Username = userDto.username,
        Password = userDto.password,
        Firstname = userDto.firstname,
        Lastname = userDto.lastname,
        Brithday = birthday,
        DNI = userDto.dni,
        Phone = userDto.phone,
        Email = userDto.email,
        Photo = userDto.photo,
        FK_role = role,
        Verification = verification,
        Is_active = is_active,
        Created_at = createAt,
        Update_at = updateAt,
        Token = token
    });

    return;
}).Produces(StatusCodes.Status200OK)
.WithName("Login").WithTags("Accounts");
```


Por la presente se concede permiso, sin cargo, a cualquier persona que obtenga acceso
al sistema OlympiaCloud para fines comunitarios sin que perjudique la integridad de
ninguna institución y/o empresa registrada o asociada a nuestro sistema.
Los usuario estaran sujetos a las siguientes cláusulas:

1. Acceso: Todo usuario debera introducir información personal, otorgando el derecho
a al sistema de manejar dicha información para fines de apoyar a los procesos que se
emplean en dicha plataforma de servicios.

2. Permisos: Todo usuario tendrá el acceso a información relevante a su cuenta, más sin
emgargo, las instituciones a fines de colaborar con la resolución de incidencias,
tambien obtendrán acceso a dicha información, siempre y cuando sea relevante a su asunto.

3. Licencia: Este sistema no podrá ser replicado, copiado y/o distribuido de forma gratuita,
WECREATIVE DIGITAL con R.U.C. 1-720-2345 y D.V. 40, se reserva todos los derechos intelectuales
y de creación.

EL SISTEMA SE PROPORCIONA "TAL CUAL", SIN GARANTÍA DE NINGÚN TIPO, EXPRESA O
IMPLÍCITO, INCLUYENDO PERO NO LIMITADO A LAS GARANTÍAS DE COMERCIABILIDAD,
IDONEIDAD PARA UN PROPÓSITO PARTICULAR Y NO VIOLACIÓN. EN NINGÚN CASO LA
LOS AUTORES O TITULARES DE LOS DERECHOS DE AUTOR SERÁN RESPONSABLES DE CUALQUIER RECLAMACIÓN, DAÑOS U OTROS
TIPOS DE RESPONSABILIDAD, YA SEA EN UNA ACCIÓN DE CONTRATO, AGRAVIO O DE OTRA FORMA, DERIVADA DE,
FUERA DE O EN CONEXIÓN CON EL SISTEMA O EL USO U OTROS TRATOS EN EL
SISTEMA.

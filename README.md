# koa-passport-cas

Cas authentication strategies for Passport.

#### Configure Strategy

    passport.use(new (require('passport-cas').Strategy)({
      ssoBaseURL: 'http://www.example.com/',
      serverBaseURL: 'http://localhost:3000'
    }, function(login, done) {
      User.findOne({login: login}, function (err, user) {
        if (err) {
          return done(err);
        }
        if (!user) {
          return done(null, false, {message: 'Unknown user'});
        }
        return done(null, user);
      });
    }));

#### Authenticate Requests

    passport.authenticate('cas', function *(err, user, info) {
      if (err) {
        return next(err);
      }

      if (!user) {
        req.session.messages = info.message;
        return res.redirect('/');
      }

      req.logIn(user, function (err) {
        if (err) {
          return next(err);
        }

        req.session.messages = '';
        return res.redirect('/');
      });
    })

Fixed some bugs for koa in [passport-cas](https://github.com/sadne/passport-cas).

## License

[The MIT License](http://opensource.org/licenses/MIT)
